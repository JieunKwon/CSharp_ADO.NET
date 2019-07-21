# CSharp_ADO.NET

@ March 2019

@ ActiveX Data Objects for .NET

@ A set of classes that expose data access services for .NET  


Two Main Componants 
---------------
1. Data Set - represents the actual data

2. Data Providers - handle communication with the physical data store


Data Providers  
------------
- SQL Server: Uses the System.Data.SqlClient namespace

- OLE DB: Uses the System.Data.OleDb namespace

- ODBC: Uses the System.Data.Odbc namespace

- Oracle : uses System.Data.OracleClient namespace


Objects of Data Providers
-------------

- Conection : establish a connection to a data source

- Command : execute a command to a data source

- DataReader : reads a forward-only, read-only data from a data source

- DataAdapter : populate a data set and resolves updates with the data source


SQL-Server Data Provider Code Example
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


    using System;
    using System.Data;
    using System.Data.SqlClient;

    class Program
    {
        static void Main()
        {
            string connectionString =
                "Data Source=(local);Initial Catalog=Northwind;"
                + "Integrated Security=true";

        // Provide the query string with a parameter placeholder.
        string queryString =
            "SELECT ProductID, UnitPrice, ProductName from dbo.products "
                + "WHERE UnitPrice > @pricePoint "
                + "ORDER BY UnitPrice DESC;";

        // Specify the parameter value.
        int paramValue = 5;

        // Create and open the connection in a using block. This
        // ensures that all resources will be closed and disposed
        // when the code exits.
        using (SqlConnection connection =
            new SqlConnection(connectionString))
        {
            // Create the Command and Parameter objects.
            SqlCommand command = new SqlCommand(queryString, connection);
            command.Parameters.AddWithValue("@pricePoint", paramValue);

            // Open the connection in a try/catch block. 
            // Create and execute the DataReader, writing the result
            // set to the console window.
            try
            {
                connection.Open();
                SqlDataReader reader = command.ExecuteReader();
                while (reader.Read())
                {
                    Console.WriteLine("\t{0}\t{1}\t{2}",
                        reader[0], reader[1], reader[2]);
                }
                reader.Close();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
            Console.ReadLine();
        }
    }
