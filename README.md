//////////////


<Window x:Class="Console1.UserControl1"

             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"

             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"

             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 

             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 

             xmlns:local="clr-namespace:Console1"

             mc:Ignorable="d" 

             d:DesignHeight="600" d:DesignWidth="900">

    <Grid Margin="0,-5,0,5">







        <ComboBox x:Name="Server" HorizontalAlignment="Left" Margin="21,41,0,0" VerticalAlignment="Top" Width="120"

             Loaded="ComboBox_Loaded" SelectionChanged="ComboBox_SelectionChanged" SelectedIndex="0" IsDropDownOpen="False" IsEditable="False" IsReadOnly="True" IsEnabled="True">

        </ComboBox>




        <GroupBox x:Name="App_Group" Header="Applications" HorizontalAlignment="Left" Margin="21,86,0,0"

                  VerticalAlignment="Top" Height="257" Width="138">

            <Grid HorizontalAlignment="Left" Height="251" VerticalAlignment="Top"

                  Width="124" Margin="10,0,0,0">

                <Grid.ColumnDefinitions>

                    <ColumnDefinition Width="34*"/>

                    <ColumnDefinition Width="5*"/>

                </Grid.ColumnDefinitions>

                <CheckBox x:Name="cb_apssia" Content="APSSIA" HorizontalAlignment="Left"

                          Margin="10,10,0,0" VerticalAlignment="Top" Height="15" Width="58" Checked="cb_apssia_Checked" Unchecked="cb_apssia_Unchecked"/>

                <CheckBox x:Name="cb_apsia" Content="APSIA" HorizontalAlignment="Left"

                          Margin="10,30,0,0" VerticalAlignment="Top" Height="15" Width="52" Checked="cb_apsia_Checked"/>

                <CheckBox x:Name="cb_sia" Content="SIA" HorizontalAlignment="Left"

                          Margin="10,50,0,0" VerticalAlignment="Top" Height="15" Width="37"/>

                <CheckBox x:Name="cb_sro" Content="SRO" HorizontalAlignment="Left"

                          Margin="10,70,0,0" VerticalAlignment="Top" Height="15" Width="42"/>

                <CheckBox x:Name="cb_biztalk" Content="BIZTALK" HorizontalAlignment="Left"

                          Margin="10,90,0,0" VerticalAlignment="Top" Height="15" Width="63"/>

                <CheckBox x:Name="cb_property" Content="PROPERTY" HorizontalAlignment="Left"

                          Margin="10,110,0,0" VerticalAlignment="Top" Height="15" Width="75"/>

                <CheckBox x:Name="cb_homes" Content="HOMES" HorizontalAlignment="Left"

                          Margin="10,130,0,0" VerticalAlignment="Top" Height="15" Width="61"/>

                <CheckBox x:Name="cb_boats" Content="BOATS" HorizontalAlignment="Left"

                          Margin="10,150,0,0" VerticalAlignment="Top" Height="15" Width="55"/>

                <CheckBox x:Name="cb_comprater" Content="COMPRATER" HorizontalAlignment="Left"

                          Margin="10,171,0,0" VerticalAlignment="Top" Height="15" Width="87"/>

                <CheckBox x:Name="cb_all" Content="ALL" HorizontalAlignment="Left"

                          Margin="10,192,0,0" VerticalAlignment="Top" Height="15" Width="39"/>

            </Grid>

        </GroupBox>

        <GroupBox x:Name="Silo_Group" Header="Silo's" HorizontalAlignment="Left" Margin="182,86,0,0"

                  VerticalAlignment="Top" Height="135" Width="92">

            <Grid HorizontalAlignment="Left" Height="112" VerticalAlignment="Top"

                  Width="76" Margin="10,0,-6,0" RenderTransformOrigin="0.572,0.511">

                <Grid.ColumnDefinitions>

                    <ColumnDefinition Width="41*"/>

                    <ColumnDefinition Width="5*"/>

                </Grid.ColumnDefinitions>

                <CheckBox x:Name="cb_s1" Content="silo1" HorizontalAlignment="Left"

                          Margin="9,12,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

                <CheckBox x:Name="cb_s2" Content="silo2" HorizontalAlignment="Left"

                          Margin="10,32,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

                <CheckBox x:Name="cb_s3" Content="silo3" HorizontalAlignment="Left"

                          Margin="10,52,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

                <CheckBox x:Name="cb_s4" Content="silo4" HorizontalAlignment="Left"

                          Margin="10,72,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

                <CheckBox x:Name="cb_sall" Content="all" HorizontalAlignment="Left"

                          Margin="10,92,0,0" VerticalAlignment="Top" Height="15" Width="32"/>

            </Grid>

        </GroupBox>

        <GroupBox x:Name="Dev_Group" Header="Region" HorizontalAlignment="Left" Margin="182,226,0,0"

                  VerticalAlignment="Top" Height="117" Width="92">

            <Grid HorizontalAlignment="Left" Height="112" VerticalAlignment="Top"

                  Width="76" Margin="10,0,-6,0" RenderTransformOrigin="0.572,0.511">

                <Grid.ColumnDefinitions>

                    <ColumnDefinition Width="41*"/>

                    <ColumnDefinition Width="5*"/>

                </Grid.ColumnDefinitions>

                <CheckBox x:Name="cb_int" Content="INT" HorizontalAlignment="Left"

                          Margin="9,12,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

                <CheckBox x:Name="cb_dev" Content="DEV" HorizontalAlignment="Left"

                          Margin="10,32,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

                <CheckBox x:Name="cb_drall" Content="ALL" HorizontalAlignment="Left"

                          Margin="10,52,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

            </Grid>

        </GroupBox>

        <GroupBox x:Name="Test_Group" Header="Region" HorizontalAlignment="Left" Margin="182,226,0,0"

                  VerticalAlignment="Top" Height="117" Width="92">

            <Grid HorizontalAlignment="Left" Height="112" VerticalAlignment="Top"

                  Width="76" Margin="10,0,-6,0" RenderTransformOrigin="0.572,0.511">

                <Grid.ColumnDefinitions>

                    <ColumnDefinition Width="41*"/>

                    <ColumnDefinition Width="5*"/>

                </Grid.ColumnDefinitions>

                <CheckBox x:Name="cb_sys" Content="SYS" HorizontalAlignment="Left"

                          Margin="9,12,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

                <CheckBox x:Name="cb_uat" Content="UAT" HorizontalAlignment="Left"

                          Margin="10,32,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

                <CheckBox x:Name="cb_trall" Content="ALL" HorizontalAlignment="Left"

                          Margin="10,52,0,0" VerticalAlignment="Top" Height="15" Width="44"/>

            </Grid>

        </GroupBox>

        <Label Content="Choose Server" HorizontalAlignment="Left" Margin="21,10,0,0" VerticalAlignment="Top" Width="90" Cursor="Cross"/>

        <Button Content="Submit"

                HorizontalAlignment="Left"

                Margin="36,368,0,0"

                VerticalAlignment="Top"

                Width="86"

                Click="Button_Click"/>

        <Button Content="Close"

                HorizontalAlignment="Left"

                Margin="180,368,0,0"

                VerticalAlignment="Top"

                Width="86" Click="Button_Click_1"

                />

        <DataGrid x:Name="dataGrid" ItemsSource="{Binding}" HorizontalAlignment="Left" Margin="341,41,0,0" VerticalAlignment="Top" Height="103" Width="235"/>

      

    </Grid>




</Window>



//////////////////////////////////////////////////

using System;

using System.Collections.Generic;

using System.Linq;

using System.Text;

using System.Threading.Tasks;

using System.Windows;

using System.Windows.Controls;

using System.Windows.Data;

using System.Windows.Documents;

using System.Windows.Input;

using System.Windows.Media;

using System.Windows.Media.Imaging;

using System.Windows.Navigation;

using System.Windows.Shapes;

using System.Data;

using System.Collections;




namespace Console1

{

    /// <summary>

    /// Interaction logic for UserControl1.xaml

    /// </summary>

    public partial class UserControl1 : Window

    {

        ArrayList app_names = new ArrayList();

        public UserControl1()

        {

            

            InitializeComponent();

            

        }

        static DataTable GetTable()

        {

            // Here we create a DataTable with four columns.

            DataTable table = new DataTable();

            table.Columns.Add("Dosage", typeof(int));

            table.Columns.Add("Drug", typeof(string));

            table.Columns.Add("Patient", typeof(string));

            table.Columns.Add("Date", typeof(DateTime));




            // Here we add five DataRows.

            table.Rows.Add(25, "Indocin", "David", DateTime.Now);

            table.Rows.Add(50, "Enebrel", "Sam", DateTime.Now);

            table.Rows.Add(10, "Hydralazine", "Christoff", DateTime.Now);

            table.Rows.Add(21, "Combivent", "Janet", DateTime.Now);

            table.Rows.Add(100, "Dilantin", "Melanie", DateTime.Now);

            return table;

        }




        private void ComboBox_Loaded(object sender, RoutedEventArgs e)

        {

            // ... A List.

            List<string> data = new List<string>();

            data.Add("Development");

            data.Add("Test");

            data.Add("Performance");

            data.Add("ModelOffice");




            // ... Get the ComboBox reference.

            var comboBox = sender as ComboBox;




            // ... Assign the ItemsSource to the List.

            comboBox.ItemsSource = data;




            // ... Make the first item selected.

            comboBox.SelectedIndex = 0;

        }

        private void ComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)

        {

            // ... Get the ComboBox.

            var comboBox = sender as ComboBox;




            // ... Set SelectedItem as Window Title.

            string value = comboBox.SelectedItem as string;

            this.Title = "Selected: " + value;

            if (value == "Development")

            {

                Silo_Group.Visibility = System.Windows.Visibility.Visible;

                Dev_Group.Visibility = System.Windows.Visibility.Visible;

                Test_Group.Visibility = System.Windows.Visibility.Hidden;

            }

            else if (value == "Test")

            {

                Silo_Group.Visibility = System.Windows.Visibility.Visible;

                Test_Group.Visibility = System.Windows.Visibility.Visible;

                Dev_Group.Visibility = System.Windows.Visibility.Hidden;

            }

            else

            {

                Silo_Group.Visibility = System.Windows.Visibility.Hidden;

                Test_Group.Visibility = System.Windows.Visibility.Hidden;

                Dev_Group.Visibility = System.Windows.Visibility.Hidden;

            }

        }

        private void Button_Click(object sender, RoutedEventArgs e)

        {

            this.Title = "Clicked";

            DataTable table1 = GetTable();

            dataGrid.DataContext = table1.DefaultView;

            MessageBox.Show(app_names[0] as string);

        }




        private void cb_apssia_Checked(object sender, RoutedEventArgs e)

        {

            app_names.Add("apsia01");

        }

        private void cb_apssia_Unchecked(object sender, RoutedEventArgs e)

        {

            app_names.Remove("apsia01");

        }

        

        private void cb_apsia_Checked(object sender, RoutedEventArgs e)

        {

            app_names.Add("apsaiprod");

        }




        private void Button_Click_1(object sender, RoutedEventArgs e)

        {

            Application.Current.Shutdown();

        }

    }

}



///////////////////////////////////////////////////////////////////////////// 


using System;

using System.Collections.Generic;

using System.Linq;

using System.Text;

using System.Threading.Tasks;

using System.Runtime.InteropServices;

using System.Windows;

using System.Xaml;







namespace Console1

{

   public class Program

    {

        [STAThread]

        static void Main(string[] args)

        {

          //  ShowConsole(); // Show the console window (for Win App projects)

            Console.WriteLine("Opening window...");

            InitializeWindows(); // opens the WPF window and waits here

            Console.WriteLine("Exiting main...");

        }




        [DllImport("kernel32.dll", SetLastError = true)]

        static extern bool AllocConsole(); // Create console window




        [DllImport("kernel32.dll")]

        static extern IntPtr GetConsoleWindow(); // Get console window handle




        [DllImport("user32.dll")]

        static extern bool ShowWindow(IntPtr hWnd, int nCmdShow);




        const int SW_HIDE = 0;

        const int SW_SHOW = 5;




        public static Application WinApp { get; private set; }

        public static Window UserControl1 { get; private set; }

        static void InitializeWindows()

        {

            WinApp = new Application();

            WinApp.Run(UserControl1 = new UserControl1()); // note: blocking call

        }

        static void ShowConsole()

        {

            var handle = GetConsoleWindow();

            if (handle == IntPtr.Zero)

                AllocConsole();

            else

                ShowWindow(handle, SW_SHOW);

        }




        static void HideConsole()

        {

            var handle = GetConsoleWindow();

            if (handle != null)

                ShowWindow(handle, SW_HIDE);

        }

        

    }

}



/////////////////////////////////////////////////////////////// 

