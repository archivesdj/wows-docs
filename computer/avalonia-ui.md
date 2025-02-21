# Avalonia UI

Avalonia는 .NET 기반의 크로스 플랫폼 UI 프레임워크로, Windows, Linux, macOS 등 다양한 플랫폼에서 동작할 수 있는 사용자 인터페이스를 개발하는 데 사용됩니다. Avalonia는 XAML과 C#을 사용하여 UI를 설계하고 구현할 수 있도록 지원합니다.

Avalonia는 WPF(Windows Presentation Foundation)와 비슷한 구조를 가지고 있으며, XAML을 사용하여 사용자 인터페이스를 정의하고 코드 비하인드(C# 코드)를 통해 이를 처리할 수 있습니다. 이는 .NET 플랫폼에서 익숙한 개발자들에게 편의성을 제공합니다.

주요 특징 및 장점:

1. **크로스 플랫폼 지원:** Avalonia는 Windows, Linux, macOS에서 모두 동작할 수 있는 플랫폼 간 프레임워크입니다. 따라서 동일한 코드베이스로 여러 플랫폼에 대한 애플리케이션을 개발할 수 있습니다.

2. **XAML 사용:** XAML(Extensible Application Markup Language)은 사용자 인터페이스를 선언적으로 정의하기 위한 마크업 언어입니다. Avalonia에서도 XAML을 사용하여 UI를 설계할 수 있습니다.

3. **MVVM 패턴 지원:** Avalonia는 MVVM(Model-View-ViewModel) 아키텍처 패턴을 지원하여 코드를 더 효과적으로 구성하고 유지 관리할 수 있도록 합니다.

4. **스타일링 및 템플릿:** XAML을 사용하면 스타일 및 템플릿을 통해 UI 요소의 외관을 손쉽게 변경할 수 있습니다.

5. **이벤트 및 명령 시스템:** Avalonia는 이벤트 및 명령 시스템을 통해 사용자 인터페이스의 상호 작용을 처리할 수 있습니다.

Avalonia는 .NET Core 및 .NET 5 이상의 버전에서 사용할 수 있으며, 오픈 소스로 개발되고 있어 커뮤니티의 참여와 기여가 가능합니다. 자세한 정보는 [Avalonia의 공식 웹사이트](https://docs.avaloniaui.net/)나 [GitHub 페이지](https://github.com/avaloniaui/avalonia)에서 확인할 수 있습니다.

## Todo App 만들기

Avalonia를 사용하여 간단한 ToDo 애플리케이션을 만들어보겠습니다. 이 예제에서는 Avalonia 프로젝트를 생성하고, XAML을 사용하여 UI를 디자인하고, MVVM 아키텍처를 적용하여 ToDo 항목을 관리하는 간단한 애플리케이션을 만들어 보겠습니다.

1. **프로젝트 생성:**
   Avalonia 프로젝트를 생성하기 위해 새로운 .NET 프로젝트를 만듭니다. 터미널이나 명령 프롬프트에서 다음 명령을 실행합니다:

   ```bash
   dotnet new avalonia.mvvm -n AvaloniaTodoApp
   ```

   위 명령은 Avalonia MVVM 템플릿을 사용하여 "AvaloniaTodoApp"이라는 프로젝트를 생성합니다.

2. **프로젝트 폴더 이동:**
   생성된 프로젝트 폴더로 이동합니다.

   ```bash
   cd AvaloniaTodoApp
   ```

3. **ToDo 모델 클래스 추가:**
   `Models` 폴더에 `TodoItem.cs`라는 이름의 새로운 클래스를 만들어 ToDo 항목을 나타내는 모델을 정의합니다.

   ```csharp
   // TodoItem.cs
   namespace AvaloniaTodoApp.Models
   {
       public class TodoItem
       {
           public string Task { get; set; } = string.Empty;
           public bool IsCompleted { get; set; }
       }
   }
   ```

4. **ViewModel 작성:**
   `ViewModels` 폴더에 `MainWindowViewModel.cs`라는 이름의 클래스를 만들어 ToDo 애플리케이션의 ViewModel을 작성합니다.

   ```csharp
   // MainWindowViewModel.cs
   using Avalonia.Collections;
   using AvaloniaTodoApp.Models;

   namespace AvaloniaTodoApp.ViewModels
   {
       public class MainWindowViewModel : ViewModelBase
       {
           public AvaloniaList<TodoItem> TodoItems { get; } = new AvaloniaList<TodoItem>();

           public MainViewModel()
           {
               // 초기 ToDo 아이템 추가
               TodoItems.Add(new TodoItem { Task = "Sample Task 1", IsCompleted = false });
               TodoItems.Add(new TodoItem { Task = "Sample Task 2", IsCompleted = true });
           }
       }
   }
   ```

5. **MainWindow.xaml 작성:**
   `Views` 폴더에 `MainWindow.xaml`을 만들어 UI를 디자인합니다.

   ```xml
   <!-- MainWindow.xaml -->
    <Window xmlns="https://github.com/avaloniaui"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:vm="using:AvaloniaTodoApp.ViewModels"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
            x:Class="AvaloniaTodoApp.Views.MainWindow"
            x:DataType="vm:MainWindowViewModel"
            Width="300" Height="500"
            Icon="/Assets/avalonia-logo.ico"
            Title="Avalonia Todo App">

        <Design.DataContext>
            <!-- This only sets the DataContext for the previewer in an IDE,
                to set the actual DataContext for runtime, set the DataContext property in code (look at App.axaml.cs) -->
            <vm:MainWindowViewModel/>
        </Design.DataContext>

        <StackPanel>
            <ListBox ItemsSource="{Binding TodoItems}">
                <ListBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel Orientation="Horizontal">
                            <CheckBox IsChecked="{Binding IsCompleted}" VerticalAlignment="Center"/>
                            <TextBlock Text="{Binding Task}" Margin="10,0" VerticalAlignment="Center"/>
                        </StackPanel>
                    </DataTemplate>
                </ListBox.ItemTemplate>
            </ListBox>

            <TextBox Name="taskTextBox" Margin="0,10"/>
            <Button Content="Add Task" Click="AddTask_Click" Margin="0,10"/>
        </StackPanel>

    </Window>
   ```

6. **MainWindow.xaml.cs 작성:**
   `Views` 폴더에 `MainWindow.xaml.cs`를 만들어 코드 비하인드를 작성합니다.

   ```csharp
   // MainWindow.xaml.cs
   using Avalonia;
   using Avalonia.Controls;
   using Avalonia.Markup.Xaml;
   using AvaloniaTodoApp.ViewModels;

   namespace AvaloniaTodoApp.Views
   {
       public class MainWindow : Window
       {
           public MainWindow()
           {
               InitializeComponent();
           }

           private void AddTask_Click(object sender, Avalonia.Interactivity.RoutedEventArgs e)
           {
               var taskTextBox = this.FindControl<TextBox>("taskTextBox");
               var viewModel = (MainViewModel)DataContext;

               if (!string.IsNullOrWhiteSpace(taskTextBox.Text))
               {
                   viewModel.TodoItems.Add(new TodoItem { Task = taskTextBox.Text, IsCompleted = false });
                   taskTextBox.Text = "";
               }
           }
       }
   }
   ```

7. **애플리케이션 실행:**
   프로젝트를 빌드하고 실행하여 ToDo 애플리케이션을 확인합니다.

   ```bash
   dotnet run
   ```

   이제 간단한 ToDo 앱이 실행되고, 할 일 목록에 항목을 추가할 수 있습니다.

이것은 Avalonia를 사용하여 간단한 ToDo 애플리케이션을 만드는 기본적인 예제입니다. 복잡성을 높이고 더 많은 기능을 추가하려면 Avalonia 문서 및 예제를 참고하는 것이 도움이 될 것입니다.