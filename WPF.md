# 布局基础

1. Grid.RowDefinitions 定义行 高度 Height="auto" 自适应 Height="100" 给定宽度  Height="2*" 按比例 第一行是第二行的两倍 
2. Grid.ColumnDefinitions 定义列
3. Grid.ColumnSpan 跨行
4.  Grid.RowSpan 跨列
5. StackPanel 局部布局 Orientation="Horizontal"  水平排列  Orientation="Vertical" 垂直排列
6. WrapPanel 局部布局 默认水平排列  Orientation="Horizontal"  水平排列  Orientation="Vertical" 垂直排列（与StackPanel区别 WrapPanel 显示所有的元素， StackPanel 会隐藏）
7. DockPanel 设置元素的方向  LastChildFill ="False" 最后一个元素舒服沾满  DockPanel.Dock="Left" 指定方向
8. UniformGrid 在有限的空间里面，平均排列元素

```
<Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*"></RowDefinition>
            <RowDefinition ></RowDefinition>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition></ColumnDefinition>
            <ColumnDefinition></ColumnDefinition>
        </Grid.ColumnDefinitions>
		<!--
        <Border Grid.Row="0" Grid.Column="0" Background="Red"></Border>
        <Border Grid.Row="0" Grid.Column="1" Background="Yellow"></Border>
        <Border Grid.Row="1" Grid.Column="0" Background="Blue"></Border>
        <Border Grid.Row="1" Grid.Column="1" Background="Green"></Border>
        --!>
        <!--
         <Border Background="Red" Grid.ColumnSpan="2"  Grid.RowSpan="2" ></Border>
        --!>
        <StackPanel Orientation="Vertical" >
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
        </StackPanel>
        
        <WrapPanel Grid.Row="1">
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
            <Button Width="100" Height="40"></Button>
        </WrapPanel>
        
        <DockPanel LastChildFill="False">
            <Button Width="100" Height="40" DockPanel.Dock="Left"></Button>
            <Button Width="100" Height="40" DockPanel.Dock="Right"></Button>
            <Button Width="100" Height="40" DockPanel.Dock="Top"></Button>
            <Button Width="100" Height="40" DockPanel.Dock="Bottom"></Button>
        </DockPanel>
        
        <UniformGrid Rows="3" Columns="2">
            <Button></Button>
            <Button></Button>
            <Button></Button>

            <Button></Button>
            <Button></Button>
            <Button></Button>
        </UniformGrid>
    </Grid>
```

# 样式

1. TargetType 直接类型
2. x:Key="buttonstyle" 定义样式类名
3. Style="{StaticResource buttonstyle}" 使用样式
3. BasedOn 继承样式 （TargetType 保持一样才能继承）

```
<Window x:Class="dome.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:dome"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Window.Resources>
        <Style x:Key="buttonstylez" TargetType="Button">
            <Setter Property="FontSize" Value="18"></Setter>
        </Style>

        <Style x:Key="buttonstyle" TargetType="Button" BasedOn="{StaticResource buttonstylez} ">
            <Setter Property="Foreground" Value="Black"></Setter>
            <Setter Property="Background" Value="Yellow"></Setter>
            <Setter Property="Content" Value="button2"></Setter>
        </Style>
    </Window.Resources>
    <Grid>
        <StackPanel>
            <Button Style="{StaticResource buttonstyle}"></Button>
            <Button Style="{StaticResource buttonstyle}"></Button>
            <Button Style="{StaticResource buttonstyle}"></Button>
        </StackPanel>
    </Grid>
</Window>


```

