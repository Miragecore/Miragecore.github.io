---
title : ReactiveUI를 이용한 간단한 Databinding
tags : wpf ReavtiveUI Databinding 
---

### Install
NugetPackage를 이용하여 설치
 - reactiveUI.wpf
 - ReactiveUI.fody
 - Fody Package를 4.2.1로 다운그레이드

### ViewModel 생성

```csharp
using ReactiveUI;
using ReactiveUI.Fody.Helpers;   //[Reactive] Keyword를 쓰기 위해 

public class MainViewModel : reactiveObject
{    
    public ReactiveCommand<Unit, Unit> TestCmd { get; }    
    
    //[Reactive] keyword를 씀으로 인해 Dependecy Property등 많은 코드를 쓰지 않아도 되게 해준다.
    [Reactive] public string szTitle {get; set;}

    //[Reactive] Keyword를 사용하지 않을 경우 아래와 같이 해야 함.
    private string theText;
    public string TheText
    {
        get => theText;
        set => this.RaiseAndSetIfChanged(ref this.theText, value);
    }

    //List와 같은 Collection을 Binding해야 할 경우는 
    //DynamicData의 SourceList나 SourceCache를 이용해야 한다.

    public MainViewModel()
    {
        TestCmd = ReactiveCommand.Create(() => Console.WriteLine("Test"));    
    }
}
```

### View behind code 수정

```csharp
using ReactiveUI;
public partial class MainWindow : ReactiveWindow<MainViewModel>
{
    public MainWindow()
    {
        InitializeComponent();
        ViewModel = new MainViewModel();

        //Reactive 프로그래밍에서 WhenXXX 이런류의 것들이 많이 있던데
        //늘 그렇지만 쓰기에 따라 UI쪽에서는 많이 편해질것 같다.
        this.WhenActivated(d =>
        {
            //command bind
            this.BindCommand(ViewModel,
                vm => vm.TestCmd, vw => vw.btn_Test
                ).DisposeWith(d);

            //button contents binding
            this.OneWayBind(ViewModel,
                    vm => vm.szTitle,
                    vw => vw.btn_Test.content);

            //textblock binding
            this.OneWayBind(this.ViewModel, x => x.TheText, x => x.TheTextBlock.Text)
                .DisposeWith(d);
        });
    }
}
```

### View XML 수정
```xml
<rxui:ReactiveWindow  
    x:Class="Sample.WorkingWindow"
    x:TypeArguments="vm:WorkingWindowViewModel"
    xmlns:vm="clr-namespace:ViewModels"
    xmlns:rxui="http://reactiveui.net"
    ...>
    <stackpanel>
        <TextBlock Name="TheTextBlock"/>
        <Button Name="btn_Test"/>
    </stackpanel>
</rxui:ReactiveWindow>
```

View와 ViewModel을 이용하여 View를 독립적으로 동작하도록 하였다.
그렇다면 이 View들간의 관계를 어떻게 조직적으로 구성할 것인가?

```
public RoutingState Router { get; private set; }
IMutableDependencyResolver dependencyResolver;
```
위와 같은 녀석들이 있긴 하던데 MVVM은 으으.. 어렵다.