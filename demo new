using System.Text;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;
using System;
using System.Collections.ObjectModel;
using Microsoft.Data.SqlClient;
using System.Data;

namespace demo
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        private readonly string connectionString = "Data Source=DESKTOP-KS66D61;Initial Catalog=demo;Integrated Security=True;TrustServerCertificate=True;";

        public ObservableCollection<Material_products__import> DataList { get; set; } = new ObservableCollection<Material_products__import>();

        public MainWindow()
        {
            InitializeComponent();
            DataContext = this; 

            LoadData(); 
        }
      public class Material_products__import
            {
                public string Materials_name { get; set; }
                public string Product { get; set; }
                public string Value_Of_Material { get; set; }
            }
        }

        private async void LoadData()
        {
            try
            {
                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    await connection.OpenAsync();
                    string asd = "Value_Of_Material";
                    double number = 1;
                    //string sqlQuery = "SELECT Materials_name, Product, Value_Of_Material FROM Material_products__import$";
                    string sqlQuery1 = "SELECT Materials_name FROM Material_products__import$ WHERE " + asd + " = " + number;
                    SqlCommand command = new SqlCommand(sqlQuery1, connection);
                    SqlDataReader reader = await command.ExecuteReaderAsync();

                    DataList.Clear(); 

                    while (await reader.ReadAsync())
                    {
                        string MaterialsName = reader.GetString(0);
                        string SProduct = reader.GetString(1);
                        //double ValueOfMaterial = reader.GetFloat(2);
                        DataList.Add(new Material_products__import { Materials_name = MaterialsName, Product = SProduct/*, Value_Of_Material = ValueOfMaterial.ToString()*/ });
                    }
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Ошибка при загрузке данных: {ex.Message}");
            }
        }

        private async void AddData_Click(object sender, RoutedEventArgs e)
        {
            string name = NameTextBox.Text;  
            string value = ValueTextBox.Text;
            string id = ValueTextBox.Text;

            if (!int.TryParse(ValueTextBox.Text, out int value))
            {
                MessageBox.Show("Некорректное значение для Value.");
                return;
            }

            try
            {
                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    await connection.OpenAsync();

                    string sqlQuery = "INSERT INTO Material_products__import$ (Name, Value) VALUES (@Name, @Value); SELECT SCOPE_IDENTITY();"; 
                    SqlCommand command = new SqlCommand(sqlQuery, connection);
                    command.Parameters.AddWithValue("@Name", name);
                    command.Parameters.AddWithValue("@Value", value);

                    var newId = await command.ExecuteScalarAsync();
                    if (newId != null)
                    {
                        // int id = Convert.ToInt32(newId);
                        DataList.Add(new Material_products__import { Materials_name = id, Product = name, Value_Of_Material = value });
                        MessageBox.Show("Данные успешно добавлены!");
                    }
                    else
                    {
                        MessageBox.Show("Не удалось добавить данные.");
                    }

                }
            }
            catch (Exception ex)
            {
                MessageBox.Show($"Ошибка при добавлении данных: {ex.Message}");
            }
        }   
