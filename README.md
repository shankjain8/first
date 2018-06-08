import xml.etree.ElementTree as etree
import sys
import os
import json
from shutil import copyfile
v_global=""
v_array=""
xml_path=[]
main_key=0
key=0
sample_count=sys.argv[2]
file_name=sys.argv[1]
version=sys.argv[3]
temp_field=[]
list_parent=[]
prev_v_global=""


if os.path.isfile("record_format.txt"):
  print("Append Run [Version == "+version+"]")
else:
  print ("First Run [Version == "+version+"]")



if not os.path.exists("./version/"+version):
    try:
        print("Creating Version Directory == /version/"+version)
        os.makedirs("./version/"+version)
    except OSError as exc: # Guard against race condition
        if exc.errno != errno.EEXIST:
            raise




def extract_attributes(att,v_globalf,main_keyf,keyf):
  for i in att:
    j=i
    if "}" in j:
      j=i.split('}',1)[1]
    xml_path.append(v_globalf+"/"+j)


def create_field(field):

  global temp_field
  if "/" in field:
    head, tail = os.path.split(field)
  elif "/" not in field:
    tail = field

  if tail not in temp_field:
    temp_field.append(tail)
    return(tail)
  elif tail in temp_field:
    if "/" in field:
      xyz=rreplace(field,"/","_",1)
    else:
      xyz=field+"_1"
    c=create_field(xyz)
    return(c)

class InvalidMessageException(Exception):
     pass

class InvalidFieldException(Exception):
     pass


def rreplace(s, old, new, occurrence):
  li = s.rsplit(old, occurrence)
  return new.join(li)

def extract_path(xml_file,prev_v_global):
  global v_global
  global list_parent
  v_global=""
  for event, elem in etree.iterparse(xml_file, events=('start', 'end')):
    if event == 'start':
      IND="START"
      tag=elem.tag
      if "}" in elem.tag:
        tag=elem.tag.split('}', 1)[1]
      v_global=v_global+"/"+tag
      if v_global == prev_v_global:
        list_parent.append(v_global)
        list_parent.append(v_global)
      xml_path.append(v_global)
      if len(elem.attrib) != 0 :
        extract_attributes(elem.attrib,v_global,main_key,key)
      prev_v_global=v_global
    elif event == 'end':
      if IND!="START":
        list_parent.append(v_global)
      tag1=elem.tag
      if "}" in elem.tag:
        tag1=elem.tag.split('}', 1)[1]
      v_global = v_global[:-len(tag1)-1]
      IND="END"

sampling=0
file_object  = open(file_name,'r')
for file_content in file_object:
  temp_xml = open("temp", "w")
  temp_xml.write(file_content)
  temp_xml.close()
  v_global=""
  if sampling < int(sample_count):
    try:
      x = etree.fromstring(file_content)
      extract_path('temp',prev_v_global)
      os.remove("temp")
    except:
      pass
  else:
    break
  sampling=sampling+1

unique_list_parent=list(set(list_parent))
prev_path=""
end_parent=""
base_elements=[]
base_elements.append("/"+unique_list_parent[0].split('/')[1])

for i in unique_list_parent:
  head, tail = os.path.split(i)
  end_parent="unset"
  prev_path=""
  for j in list_parent:
    if ( prev_path == j and end_parent != "set" ):
      base_elements.append(i)
    if ( j == head ):
      end_parent="set"
    if ( i == j ):
      prev_path=i
      end_parent="unset"

base_elements=list(set(base_elements))
base_elements.sort(key=len)
base_elements=list(reversed(base_elements))
xml_path=list(set(xml_path))
xml_path.sort()

for c in unique_list_parent:
  xml_path.remove(c)


#CREATE UPDATED TABEL DICTIONARY
#p_table_dictionary should be read from prev_config file#
if os.path.isfile("base_dict.txt"):
  prev_base_dict = json.load(open('base_dict.txt'))
  curr_base_dict = json.load(open('base_dict.txt'))
else:
  prev_base_dict = {}
  curr_base_dict = {}
temp_field=prev_base_dict.values()


for j in base_elements:
  if prev_base_dict.get(j, None) is None:
    curr_base_dict[j]=create_field(j.lower()).replace(".","_")


base_dict  = open("base_dict.txt", "w")
base_dict.write(json.dumps(curr_base_dict)+'\n')
base_dict.close()

new_base_elements=curr_base_dict.keys()
new_base_elements=list(set(new_base_elements))
new_base_elements.sort(key=len)
new_base_elements=list(reversed(new_base_elements))

if os.path.isfile("record_format.txt"):
  prev_data_dict = json.load(open('record_format.txt'))
  curr_data_dict = json.load(open('record_format.txt'))
else:
  prev_data_dict={}
  curr_data_dict={}

record_format_list={}
n_record_format_list={}
v_record_format={}
bkp_xml_path=xml_path
for k in base_elements:
  if prev_base_dict.get(k, None) is None:
    v_record_format={}
    xml_path=bkp_xml_path
    bkp_xml_path=[]
    temp_field=[]
    for n in xml_path:
      if k in n:
        fName=create_field(n.replace(k+"/","").lower()).replace(".","_")
        v_record_format[fName]=n
      else:
        bkp_xml_path.append(n)
    record_format_list[k]=v_record_format
    curr_data_dict[k]=v_record_format
  else :
    v_record_format=curr_data_dict[k]

    xml_path=bkp_xml_path
    bkp_xml_path=[]
    temp_field=v_record_format.keys()
    temp_xml_field=v_record_format.values()
    for n in xml_path:

      if k in n and n not in temp_xml_field:
        fName=create_field(n.replace(k+"/","").lower()).replace(".","_")
        v_record_format[fName]=n
      elif k in n:
        no_action=True
      else:
        bkp_xml_path.append(n)
    curr_data_dict[k]=v_record_format
    record_format_list[k]=v_record_format

   
v_key=""
counter=0
base_key=[]
key_list=[]
for i in new_base_elements:
  v_key=""
  f_key=i
  counter=0
  for j in new_base_elements:
    counter=counter+1
    if j in i:
      head, tail = os.path.split(j)
      v_key=curr_base_dict[j]+"_seq_num"    
      curr_data_dict[i][v_key]=0
      key_list.append(v_key)
      f_key=f_key+"|"+v_key
  base_key.append(f_key)



base_element = open("base_element.txt", "w")
for j in base_key:
  base_element.write(j+"\n")
base_element.close()


key_list=list(set(key_list))
keys = open("keys.txt", "w")

for j in key_list:
  keys.write(j+"\n")
keys.close()

#print record_format_list
record_format = open("record_format.txt", "w")
record_format.write(json.dumps(curr_data_dict)+'\n')
record_format.close()


#BACKING UP THE CONFIG FILES
print("Schema Generated. Backing Up the config in ./version/"+version)
copyfile("base_element.txt","./version/"+version+"/base_element.txt")
copyfile("keys.txt","./version/"+version+"/keys.txt")
copyfile("record_format.txt","./version/"+version+"/record_format.txt")
copyfile("base_dict.txt","./version/"+version+"/base_dict.txt")



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

