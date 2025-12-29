# WPF 정리

WPF = XAML + Direct X 벡터 그래픽

MVVM 장점
View model의 사용으로
1. 테스트 쉬움.
2. UI단과 로직단이 분리되어 재사용성 증가

## ContentControl 과 DataTemplate
### 1. ContentControl
말 그대로 **"무언가 하나를 담는 그릇"**입니다.
Content라는 속성에 어떤 객체(문자열, 숫자, 혹은 클래스 인텔리전스 객체 등)든 담을 수 있음.
그런데 해당 객체를 어떻게 그려야할지에 대한 정보는 없음

### 2. DataTemplate
**"특정 데이터 타입을 화면에 이렇게 그려라"**라고 알려주는 시각적 설계도입니다.

만약 data에 따라서 다른 DataTemplate을 주고싶으면 DataTemplateSelector 를 상속한 클래스를 구현하여 사용

ex)
<Window.Resources>
    <DataTemplate DataType="{x:Type vms:HomeViewModel}">
        <views:HomeView />
    </DataTemplate>

    <DataTemplate DataType="{x:Type vms:SettingViewModel}">
        <views:SettingView />
    </DataTemplate>
</Window.Resources>

<ContentControl Content="{Binding CurrentViewModel}" ContentTemplate="요거사용해도 됨"/>
.
ContentTemplate 속성을 직접 건드리지 않아도, 리소스에 DataType이 지정된 DataTemplate이 있으면 WPF가 알아서 연결
이렇게 xaml을 구성하는 방식을 View Model First라고 하며 일반적으로 xaml에 view model을 부여하는 방식을 View First 방식이라 함


## Command
### 1.Command 란
버튼 클릭같은 UI 이벤트를 code behind가 아닌 view model과 연결하는 다리 역할.
ICommand 인터페이스를 이용, CanExecute, Execute 메서드 필요.
RelayCommand라는 ICommand 구현체를 만들어 사용

### 2. 특징
CanExecute로 버튼 활성/비활성도 자동 제어해 UI 상태를 로직과 연동도 가능

### 3. RoutedCommand vs RelayCommand
1) RoutedCommand - WPF UI 트리 안에서 흘러다니는 명령 신호. UI 컨트롤간의 
<Button Command="ApplicationCommands.Copy" CommandTarget="{Binding myText}"/>
   - 용도: 메뉴/키보드 단축키 등 표준 명령어 (ApplicationCommand.Cut 등)
   - 구현방식: CommandBinding으로 Execute/CanExecute 이벤트 핸들러 지정. Target 지정 가능
   - 라우팅	UI 요소 트리를 타고 Binding 찾음 (버블링/터널링)
   - 코드 비하인드 필요해 MVVM 깨짐
ex)
public static readonly RoutedCommand SaveCommand =  new RoutedCommand("Save", typeof(MainWindow));
// XAML 연결
<Button Command="{x:Static local:MainWindow.SaveCommand}"/>

// Code-behind (Window)
private void OnSaveExecuted(object sender, ExecutedRoutedEventArgs e) => Save();
private void OnSaveCanExecute(object sender, CanExecuteRoutedEventArgs e) => e.CanExecute = true;

<Window.CommandBindings>
  <CommandBinding Command="{x:Static local:MainWindow.SaveCommand}"
                  Executed="OnSaveExecuted"
                  CanExecute="OnSaveCanExecute"/>
</Window.CommandBindings>


3) RelayCommand - ViewModel이 직접 메서드를 노출하는 단순 커맨드 래퍼
<Button Command="{Binding SaveCmd}"/>
public ICommand SaveCmd => new RelayCommand(() => Data.Save()); 
    - 용도: ViewModel 메서드를 버튼 등에 직접 바인딩. 코드 비하인드 최소화
    - 구현방식: 생성자에 람다/델리게이트 넣기만 함. new RelayCommand(() => Save())
    - 라우팅: 라우팅 없음. 직접 Execute 호출

## 

1. Behaviors (Interaction.Triggers)
2. Attached Properties
3. ResourceDictionary + MergedDictionaries
4. ItemsControl / DataGrid 커스터마이징
5. Async Command (Task 기반)
