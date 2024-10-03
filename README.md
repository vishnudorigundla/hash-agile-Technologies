# hash-agile-Technologies
## Problem Statement :
Function Definitions
createCollection(p_collection_name)
Using Any of the programming language implement below functions
indexData(p_collection_name, p_exclude_column):
Index the given employee data into the specified collection, excluding the column provided in p_exclude_column.
searchByColumn(p_collection_name, p_column_name, p_column_value):
Search within the specified collection for records where the column p_column_name matches the value p_column_value.
getEmpCount(p_collection_name)
delEmpById(p_collection_name, p_employee_id)
•  getDepFacet(p_collection_name):
Retrieve the count of employees grouped by department from the specified collection.

Function Executions

Var v_nameCollection = ‘Hash_<Your Name>’
Once the functions are implemented, execute the functions in the given order with the parameters mentioned
Var v_phoneCollection =’Hash_<Your Phone last four digits’
createCollection(v_nameCollection)
createCollection(v_phoneCollection)
getEmpCount(v_nameCollection)
indexData(v_nameCollection,’Department’)
indexData(v_ phoneCollection, ‘Gender’)
delEmpById (v_ nameCollection ,‘E02003’)
getEmpCount(v_nameCollection)
searchByColumn(v_nameCollection,’Department’,’IT’)
searchByColumn(v_nameCollection,’Gender’ ,’Male’)
searchByColumn(v_ phoneCollection,’Department’,’IT’)
getDepFacet(v_ nameCollection)
getDepFacet(v_ phoneCollection)
## program :
```
Name : Dorigundla vishnuvardhan reddy
Ref.No: 212221230023

```
```
from elasticsearch import Elasticsearch, helpers

# Connect to Elasticsearch with more specific parameters.
# Added error handling for SniffingError
try:
    es = Elasticsearch(["http://localhost:9200"],sniff_on_start=True,sniff_on_connection_fail=True,sniffer_timeout=60)
    
    if es.ping():
        print("Successfully connected to Elasticsearch!")
    else:
        print("Failed to connect to Elasticsearch.")
except Exception as e:
    print(f"An error occurred during connection: {e}")
def createCollection(p_collection_name):
    if not es.indices.exists(index=p_collection_name):
        es.indices.create(index=p_collection_name)
        print(f"Index '{p_collection_name}' created.")
    else:
        print(f"Index '{p_collection_name}' already exists.")

def indexData(p_collection_name, p_exclude_column):
    employee_data = [
        {"EmployeeID": "E02001", "Name": "Alice", "Department": "HR", "Gender": "Female"},
        {"EmployeeID": "E02002", "Name": "Bob", "Department": "IT", "Gender": "Male"},
        {"EmployeeID": "E02003", "Name": "Charlie", "Department": "Finance", "Gender": "Male"},
        # Add more sample employee data as needed
    ]
    actions = []
    for employee in employee_data:
        doc = {key: value for key, value in employee.items() if key != p_exclude_column}
        action = {
            "_index": p_collection_name,
            "_id": employee["EmployeeID"],
            "_source": doc
        }
        actions.append(action)
    
    helpers.bulk(es, actions)
    print(f"Data indexed into '{p_collection_name}', excluding column '{p_exclude_column}'.")

def searchByColumn(p_collection_name, p_column_name, p_column_value):
    body = {
        "query": {
            "match": {
                p_column_name: p_column_value
            }
        }
    }
    res = es.search(index=p_collection_name, body=body)
    return res['hits']['hits']

def getEmpCount(p_collection_name):
    res = es.count(index=p_collection_name)
    print(f"Employee count in '{p_collection_name}': {res['count']}")
    return res['count']

def delEmpById(p_collection_name, p_employee_id):
    es.delete(index=p_collection_name, id=p_employee_id)
    print(f"Deleted employee with ID '{p_employee_id}' from collection '{p_collection_name}'.")

def getDepFacet(p_collection_name):
    body = {
        "size": 0,
        "aggs": {
            "by_department": {
                "terms": {
                    "field": "Department.keyword"
                }
            }
        }
    }
    res = es.search(index=p_collection_name, body=body)
    buckets = res['aggregations']['by_department']['buckets']
    for bucket in buckets:
        print(f"Department: {bucket['key']}, Count: {bucket['doc_count']}")
    return buckets

# Execute the functions as specified
v_nameCollection = 'vishnu'
v_phoneCollection = '4108'  # Replace with your phone's last four digits

createCollection(v_nameCollection)
createCollection(v_phoneCollection)

getEmpCount(v_nameCollection)

indexData(v_nameCollection, 'Department')
indexData(v_phoneCollection, 'Gender')

delEmpById(v_nameCollection, 'E02003')

getEmpCount(v_nameCollection)

# Search by column
print("Search results by Department (IT):")
print(searchByColumn(v_nameCollection, 'Department', 'IT'))

print("Search results by Gender (Male):")
print(searchByColumn(v_nameCollection, 'Gender', 'Male'))

print("Search results by Department (IT) in phone collection:")
print(searchByColumn(v_phoneCollection, 'Department', 'IT'))

# Department facets
getDepFacet(v_nameCollection)
getDepFacet(v_phoneCollection)
```
## outputs :
![image](https://github.com/user-attachments/assets/ff1e80be-4d0b-4b1b-b880-e4167cbae1ba)
![image](https://github.com/user-attachments/assets/19b14824-a4a6-4308-902e-c2b0189acb45)
![image](https://github.com/user-attachments/assets/cd86bc6c-a9f2-4be5-9967-8e2ae12e5cb4)

![image](https://github.com/user-attachments/assets/9d3ed9cf-1606-4579-bd7e-49557622f0b7)
![image](https://github.com/user-attachments/assets/91e8840c-5016-427e-b4a1-b361fb0458f0)

![image](https://github.com/user-attachments/assets/1798602a-bf67-4ab1-9945-5c611a7ad417)
![image](https://github.com/user-attachments/assets/1bc790cc-f85f-485e-a4ce-5a66b624bad3)
![image](https://github.com/user-attachments/assets/d03443ad-b18c-48a0-aea2-378c61510432)
## Result :
Hence the following requirements are satisfied and code executed successfully.
