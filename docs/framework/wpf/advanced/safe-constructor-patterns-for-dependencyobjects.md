---
title: DependencyObject 的安全建構函式模式
ms.date: 03/30/2017
helpviewer_keywords:
- constructor patterns for dependency objects [WPF]
- dependency objects [WPF], constructor patterns
- FXCop tool [WPF]
ms.assetid: f704b81c-449a-47a4-ace1-9332e3cc6d60
ms.openlocfilehash: fce17979fbd43df0496f972cac525fd79dcbfe32
ms.sourcegitcommit: 005980b14629dfc193ff6cdc040800bc75e0a5a5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2019
ms.locfileid: "70991811"
---
# <a name="safe-constructor-patterns-for-dependencyobjects"></a>DependencyObject 的安全建構函式模式
一般而言，類別建構函式不應該呼叫回呼 (例如，虛擬方法或委派)，因為建構函式可以當成衍生類別之建構函式的基底初始化來呼叫。 進入虛擬項目，可能是在任何指定物件的未完成初始化狀態中完成的。 不過，屬性系統本身會在內部呼叫並公開回呼，以做為相依性屬性系統的一部分。 簡單來說, 使用<xref:System.Windows.DependencyObject.SetValue%2A> call 設定相依性屬性值的作業, 可能會在判斷中的某處包含回呼。 基於這個理由，在建構函式的主體內設定相依性屬性值時應特別小心，如果您的類型是用來做為基底類別，這可能就會發生問題。 有一種特定模式可讓<xref:System.Windows.DependencyObject>您執行可避免相依性屬性狀態的特定問題, 以及本文所述的固有回呼。  

<a name="Property_System_Virtual_Methods"></a>   
## <a name="property-system-virtual-methods"></a>屬性系統虛擬方法  
 在設定相依性屬性值的<xref:System.Windows.DependencyObject.SetValue%2A>呼叫計算期間, 可能會呼叫下列虛擬方法或回呼: <xref:System.Windows.ValidateValueCallback>、 <xref:System.Windows.PropertyChangedCallback>、 <xref:System.Windows.CoerceValueCallback>、 <xref:System.Windows.DependencyObject.OnPropertyChanged%2A>。 這其中每一個虛擬方法或回呼，會在展開多樣化的 [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 屬性系統和相依性屬性時具有特殊用途。 如需如何使用這些虛擬項目來自訂屬性值判斷的詳細資訊，請參閱[相依性屬性回呼和驗證](dependency-property-callbacks-and-validation.md)。  
  
### <a name="fxcop-rule-enforcement-vs-property-system-virtuals"></a>FXCop 規則強制與屬性系統虛擬項目  
 如果您使用 Microsoft 工具 FXCop 做為建置流程的一部分，而且是衍生自從呼叫基底建構函式的特定 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 架構類別，或是在衍生類別中實作自己的相依性屬性，則您可能會遇到特殊的 FXCop 規則違規。 此違規的名稱字串為：  
  
 `DoNotCallOverridableMethodsInConstructors`  
  
 此規則屬於 FXCop 的預設公用規則集。 此規則可能會報告哪些內容，是透過相依性屬性系統來追蹤，此系統最終會呼叫相依性屬性系統虛擬方法。 即使依照本主題中所述的建議建構函式模式來執行，此規則違規可能還會繼續出現，因此，您可能需要在 FXCop 規則集組態中停用或隱藏該規則。  
  
### <a name="most-issues-come-from-deriving-classes-not-using-existing-classes"></a>大部分的問題均來自衍生類別，而非使用現有的類別  
 若接著會從中衍生您在其建構序列中使用虛擬方法實作的類別，就會發生此規則所報告的問題。 如果您密封類別，或是知道或強制您的類別將不會從中衍生，則此處所說明的考量和 FXCop 規則所引發的問題對您並不適用。 不過，如果您正以有意使用它們當做基底類別 (例如，如果您正在建立範本) 或可擴展控制項程式庫集的方式來撰寫類別，則應遵循此處針對建構函式所建議的模式。  
  
### <a name="default-constructors-must-initialize-all-values-requested-by-callbacks"></a>預設建構函式必須初始化回呼要求的所有值  
 類別覆寫或回呼所使用的任何實例成員 (在屬性系統虛擬函式區段中, 從清單中的回呼) 都必須在您的類別無參數的函式中初始化, 即使這些值已透過 "real" 值填滿nonparameterless 之函式的參數。  
  
 下列範例程式碼 (和後續範例) 是違反這項規則的虛擬 C# 範例，並將說明問題：  
  
```csharp  
public class MyClass : DependencyObject  
{  
    public MyClass() {}  
    public MyClass(object toSetWobble)  
        : this()  
    {  
        Wobble = toSetWobble; //this is backed by a DependencyProperty  
        _myList = new ArrayList();    // this line should be in the default ctor  
    }  
    public static readonly DependencyProperty WobbleProperty =   
        DependencyProperty.Register("Wobble", typeof(object), typeof(MyClass));  
    public object Wobble  
    {  
        get { return GetValue(WobbleProperty); }  
        set { SetValue(WobbleProperty, value); }  
    }  
    protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e)  
    {  
        int count = _myList.Count;    // null-reference exception  
    }  
    private ArrayList _myList;  
}  
```  
  
 當應用程式代碼`new MyClass(objectvalue)`呼叫時, 會呼叫無參數的函式和基類的函式。 然後它會`Property1 = object1`設定, 它會呼叫擁有`OnPropertyChanged` `MyClass` <xref:System.Windows.DependencyObject>上的虛擬方法。  覆寫是指尚未初始化的 `_myList`。  
  
 避免這些問題的方法之一是，確定回呼只會使用其他相依性屬性，而且每個這類相依性屬性都已建立預設值做為其已註冊中繼資料的一部分。  
  
<a name="Safe_Constructor_Patterns"></a>   
## <a name="safe-constructor-patterns"></a>安全的建構函式模式  
 若要在您的類別用來當做基底類別時，避免產生未完成初始化的風險，請遵循下列模式：  
  
#### <a name="parameterless-constructors-calling-base-initialization"></a>呼叫基底初始化的無參數函式  
 實作這些呼叫基底預設值的建構函式：  
  
```csharp  
public MyClass : SomeBaseClass {  
    public MyClass() : base() {  
        // ALL class initialization, including initial defaults for   
        // possible values that other ctors specify or that callbacks need.  
    }  
}  
```  
  
#### <a name="non-default-convenience-constructors-not-matching-any-base-signatures"></a>非預設 (便捷) 建構函式，不符合任何基底簽章  
 如果這些函式在初始化中使用參數來設定相依性屬性, 請先呼叫您自己的類別無參數的函式來進行初始化, 然後使用參數來設定相依性屬性。 這些可能是您的類別所定義的相依性屬性或繼承自基底類別的相依性屬性，但在這任一種情況下，請使用下列模式：  
  
```csharp  
public MyClass : SomeBaseClass {  
    public MyClass(object toSetProperty1) : this() {  
        // Class initialization NOT done by default.  
        // Then, set properties to values as passed in ctor parameters.  
        Property1 = toSetProperty1;  
    }  
}  
```  
  
#### <a name="non-default-convenience-constructors-which-do-match-base-signatures"></a>非預設 (便捷) 建構函式，其符合任何基底簽章  
 並不是使用相同的參數化呼叫基底函式, 而是再次呼叫您自己的類別「無參數」函式。 請勿呼叫基底初始設定式；您應該改為呼叫 `this()`。 然後使用傳遞的參數做為用來設定相關屬性的值，藉以重現原始的建構函式行為。 使用原始的基底建構函式文件做為指引，來判斷要設定特定參數的屬性：  
  
```  
public MyClass : SomeBaseClass {  
    public MyClass(object toSetProperty1) : this() {  
        // Class initialization NOT done by default.  
        // Then, set properties to values as passed in ctor parameters.  
        Property1 = toSetProperty1;  
    }  
}  
```  
  
#### <a name="must-match-all-signatures"></a>必須符合所有簽章  
 對於基底類型有多個簽章的情況, 您必須刻意將所有可能的簽章與您自己的函式執行進行比對, 以使用建議的模式呼叫類別無參數的函式, 然後再進一步設定屬性.  
  
#### <a name="setting-dependency-properties-with-setvalue"></a>使用 SetValue 設定相依性屬性  
 如果您要設定的屬性沒有屬性設定便利性的包裝函式, 並使用<xref:System.Windows.DependencyObject.SetValue%2A>設定值, 則適用相同的模式。 透過函式<xref:System.Windows.DependencyObject.SetValue%2A>參數傳遞的呼叫應該也會呼叫類別的無參數函式來進行初始化。  
  
## <a name="see-also"></a>另請參閱

- [自訂相依性屬性](custom-dependency-properties.md)
- [相依性屬性概觀](dependency-properties-overview.md)
- [相依性屬性的安全性](dependency-property-security.md)
