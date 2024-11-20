# Korterra Challenge Questions


***


# SQL / Stored Procedure Challenge:

### **Objective:** Create a stored procedure that retrieves customer orders from the past 30 days and calculates the total sales amount per customer.

 

### **Requirements:**

1. Write a stored procedure named `GetCustomerOrders` that takes no parameters.
2. The procedure should retrieve customer orders placed in the last 30 days.
3. For each customer, return the customer ID, name, and the total sales amount.
4. Ensure the procedure handles any potential null values gracefully (e.g., missing customer names).
#### Sample Table Structures:

`Customers`: `CustomerID`, `CustomerName`

`Orders`: `OrderID`, `CustomerID`, `OrderDate`, `OrderAmount` 
#### Expected Output:
| CustomerID | CustomerName | TotalSales |
|------------|--------------|------------|
| 1          | John Doe     | 500.00     |
| 2          | Jane Smith   | 300.00     |

 
### SQL Script:
```sql
CREATE PROCEDURE GetCustomerOrders
AS
BEGIN
    SELECT c.CustomerID, c.CustomerName, 
           SUM(COALESCE(o.OrderAmount, 0)) AS TotalSales
    FROM Orders o
    INNER JOIN Customers c ON o.CustomerID = c.CustomerID
    WHERE o.OrderDate >= DATEADD(day, -30, GETDATE())
    AND o.OrderDate IS NOT NULL
    AND c.CustomerID IS NOT NULL
    AND o.CustomerID IS NOT NULL
    GROUP BY c.CustomerID, c.CustomerName
    ORDER BY TotalSales DESC;
END
```

***
# Python / Azure Data Factory (ADF) Challenge:
#### **Objective:** 
Create a Python script that simulates part of an ADF pipeline, focusing on transforming a CSV file and uploading it to Azure Blob Storage.


#### **Requirements:**

1. Write a Python script that reads a CSV file containing sales data.
2.  Transform the sales data by adding a new column called `TotalPrice`, which is the product of `Quantity` and `UnitPrice`.
3.  Save the transformed data as a new CSV file.
4.  Upload the new CSV file to Azure Blob Storage.
### Bonus:
#### Add error handling for potential file read/write issues.
#### Assume the Azure Storage credentials are already available in environment variables.





Sample CSV Data:
| OrderID | ProductName | Quantity | UnitPrice |
|---------|-------------|----------|-----------|
| 1       | Widget A    | 10       | 20.00     |
| 2       | Widget B    | 5        | 15.00     |

 

Transformed Data:
| OrderID | ProductName | Quantity | UnitPrice | TotalPrice |
|---------|-------------|----------|-----------|------------|
| 1       | Widget A    | 10       | 20.00     | 200.00     |
| 2       | Widget B    | 5        | 15.00     | 75.00      |


## Python Script:

```python
import pandas as pd
from azure.storage.blob import BlobServiceClient
from azure.identity import DefaultAzureCredential

def read_csv(file_path):
    try:
        df = pd.read_csv(file_path)
        return df
    except FileNotFoundError:
        print(f"Error: File not found at {file_path}")
        return None
    except pd.errors.EmptyDataError:
        print("Error: The file is empty.")
        return None
    except Exception as e:
        print(f"An error occurred: {e}")
        return None


def upload_to_azure_blob_storage(container_name, blob_name, file_path):

    try:
        account_url = "https://<storageaccountname>.blob.core.windows.net"
        default_credential = DefaultAzureCredential()

        # Create the BlobServiceClient object
        blob_service_client = BlobServiceClient(account_url, credential=default_credential)
        container_client = blob_service_client.get_container_client(container=container_name)
        
        with open(file_path, "rb") as data:
            container_client.upload_blob(data)
        print(f"File {file_path} uploaded successfully to Azure Blob Storage")
        return True
    except Exception as e:
        print(f"Error uploading file to Azure Blob Storage: {e}")
        return False

#define file name, path to CSV, and output folder
file_name = "file.csv"
input_file_path = "path to csv folder"
output_file_path = "path to output folder"

# Read the CSV
csv_data = read_csv(input_file_path + file_name)

# replace None values with 0
csv_data = csv_data(None, 0)

#create column that is a product of the item quantity multiplied by the unit price
csv_data['TotalPrice'] = csv_data['Quantity'] * csv_data['UnitPrice']

output_file_name = output_file_path + "output_file.csv"

if sales_df is not None:
    print(f"Successfully read CSV at {input_file_path + file_name}.")
    sales_df.to_csv(output_file_name, index=False)
    print(f"Transformed data saved to {output_file_name}")

else:
    print(f"Dateframe is empty or non-existant.")

#define your variables for csv upload to azure blob
container_name = "container name"
blob_name = "blob name"
upload_to_azure_blob_storage(container_name, blob_name, output_file_name)
```
