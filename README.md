# Actitvity-6
using System;
using System.Data.SqlClient;
using System.Windows.Forms;

namespace YourNamespace
{
    public partial class frmCustomerDataEntry : Form
    {
        public frmCustomerDataEntry()
        {
            InitializeComponent();
        }

        private void frmCustomerDataEntry_Load(object sender, EventArgs e)
        {
            loadCustomer();
        }

        private void loadCustomer()
        {
            string strConnection = "Data Source=.\\sqlexpress;Initial Catalog=CustomerDB;Integrated Security=True";
            
            using (SqlConnection objConnection = new SqlConnection(strConnection))
            {
                try
                {
                    objConnection.Open();

                    string strCommand = "SELECT * FROM CustomerTable";
                    using (SqlCommand objCommand = new SqlCommand(strCommand, objConnection))
                    {
                        DataSet objDataSet = new DataSet();
                        SqlDataAdapter objAdapter = new SqlDataAdapter(objCommand);
                        objAdapter.Fill(objDataSet);
                        dtgCustomer.DataSource = objDataSet.Tables[0];
                    }
                }
                catch (Exception ex)
                {
                    MessageBox.Show($"Error: {ex.Message}");
                }
            }
        }

        private void btnAdd_Click(object sender, EventArgs e)
        {
            string gender = radioMale.Checked ? "Male" : "Female";
            string hobby = chkReading.Checked ? "Reading" : "Painting";
            string status = radioMarried.Checked ? "1" : "0";

            string strConnection = "Data Source=.\\sqlexpress;Initial Catalog=CustomerDB;Integrated Security=True";

            using (SqlConnection objConnection = new SqlConnection(strConnection))
            {
                try
                {
                    objConnection.Open();
                    string strCommand = "INSERT INTO CustomerTable (Name, Country, Gender, Hobby, Status) " +
                                        "VALUES (@Name, @Country, @Gender, 
                                    "@Hobby, @Status)";
                    using (SqlCommand objCommand = new SqlCommand(strCommand, objConnection))
                    {
                        objCommand.Parameters.AddWithValue("@Name", txtName.Text);
                        objCommand.Parameters.AddWithValue("@Country", cmbCountry.Text);
                        objCommand.Parameters.AddWithValue("@Gender", gender);
                        objCommand.Parameters.AddWithValue("@Hobby", hobby);
                        objCommand.Parameters.AddWithValue("@Status", status);
                    
                        objCommand.ExecuteNonQuery();
                    }

                    loadCustomer();
                }
                catch (Exception ex)
                {
                    MessageBox.Show($"Error: {ex.Message}");
                }
            }
        }
    }
}
