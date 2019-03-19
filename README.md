# CSharp_ADO.NET

@ ActiveX Data Objects for .NET

@ A set of classes that expose data access services for .NET  

Two Main Componants 
---------------
1. Data Set- represents the actual data

2. Data Providers - handle communication with the physical data store


Data Providers  
------------
- SQL Server: Uses the System.Data.SqlClient namespace

- OLE DB	: Uses the System.Data.OleDb namespace

- ODBC: Uses the System.Data.Odbc namespace

- Oracle : uses System.Data.OracleClient namespace


Reader
-------------
static void HasRows(SqlConnection connection)
{
    using (connection)
    {
        SqlCommand command = new SqlCommand(
          "SELECT CategoryID, CategoryName FROM Categories;",
          connection);
        connection.Open();

        SqlDataReader reader = command.ExecuteReader();

        if (reader.HasRows)
        {
            while (reader.Read())
            {
                Console.WriteLine("{0}\t{1}", reader.GetInt32(0),
                    reader.GetString(1));
            }
        }
        else
        {
            Console.WriteLine("No rows found.");
        }
        reader.Close();
    }
}
