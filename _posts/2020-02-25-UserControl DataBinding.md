---
title : UserControl DataBinding 
tags : C# wpf UserControl Databinding 
---


 - ListView를 데이터 바인딩하기
 ListView에 List를 데이터 바인딩하기 위해서는 ``ObservableCollection``을 사용

 ```csharp
// View & ViewModel
 //UserControl의 경우 재사용을 위해 변수를 외부에 노출시켜야 하는 경우가 많고,
 //이때, 따로 ViewModel을 분리할 경우, 외부 노출 변수를 위해 View 코드에서
 //ViewModel을 접근하여 다시 Model의 값을 읽어오는 등, 코드가 번잡해지는 경우가 많은 것 같다.

 //using system.collection.objectmodel
 public ObservableCollection<CogRectangle> Rectangles { get; set; } = new ObservableCollection<CogRectangle>();
 
 public AreaListViewCtrl()
    {
        InitializeComponent();

        DataContext = this;
    }
```

Xaml쪽에서 ListView 자체에 설정된 ItemSource와 각 GriveiwColum에 DisplayMemberBinding에 설정된
Binding값의 상하위 관계를 관심가져 볼것.
stringFormat지정 방법은 그때그때 참조할 것.
Check Box의 경우 별도의 데이터 템플릿으로 분리하여 적용시 바인딩 방식이 달라진다. 

```
    <StackPanel>
            <TextBlock Name="tbkTitle" Text="{Binding Title}"/>
            <ListView Height="Auto" 
                      Grid.Row="0"
              HorizontalAlignment="Left" 
              Name="lstArea" 
              ItemsSource="{Binding Rectangles}"    
              VerticalAlignment="Top" 
              Width="Auto"
              IsSynchronizedWithCurrentItem="True"
                      Margin="0">
                <ListView.View>
                    <GridView>
                        <GridView.Columns>
                            <GridViewColumn Width="30" Header="-">
                                <GridViewColumn.CellTemplate>
                                    <DataTemplate>
                                        <CheckBox IsChecked="{Binding Selected}"/>
                                    </DataTemplate>
                                </GridViewColumn.CellTemplate>
                            </GridViewColumn>
                        <GridViewColumn Width="50" DisplayMemberBinding="{Binding X, StringFormat={}{0:#,#.0}}" Header="X" />
                        <GridViewColumn Width="50" DisplayMemberBinding="{Binding Y, StringFormat={}{0:#,#.0}}" Header="Y" />
                        <GridViewColumn Width="50" DisplayMemberBinding="{Binding Width, StringFormat={}{0:#,#.0}}" Header="Width" />
                        <GridViewColumn Width="50" DisplayMemberBinding="{Binding Height, StringFormat={}{0:#,#.0}}" Header="Height" />
                        </GridView.Columns>
                    </GridView>
                </ListView.View>
            </ListView>
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Right">
                <Button Name="btn_AddItem" Click="Btn_AddItem_Click" Content="Add"/>
                <Button Name="btn_DelItem" Click="Btn_DelItem_Click" Content="Del"/>
            </StackPanel>
        </StackPanel>
```

