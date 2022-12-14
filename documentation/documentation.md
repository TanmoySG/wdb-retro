# Official Documentation of wunderDB

wunderDB is a JSON-based micro Document DB hosted at [wdb.tanmoysg.com](https://wdb.tanmoysg.com). ```Unified Actions API``` is used for accessing the remote API.

### Creating a Cluster

On registration, a cluster is created. A cluster can also be created by Posting a request to the API endpoint: <kbd>wdb.tanmoysg.com/register</kbd> from a REST Client like [Insomnia](https://insomnia.rest/) or [Postman](https://www.postman.com/) with the following JSON Data.

```
{  
    "name" : "name of the user",
    "username" : "email of the user",
    "password" : "password of user" 
}
```

On successfull registration, a Cluster ID and 3 Access Tokens are generated. These are used for accessing the API.

### Accessing the Cluster - Endpoint

The cluster can be accessed using the ```Unified Actions API```. To consume this API, use the following endpoint :
```
wdb.tanmoysg.com/connect?cluster=<cluster-id>&token=<one-of-the-three-tokens-generated>
```
The operations on this API are facilitated through ```Actions``` .

### Actions & Payloads

Actions and Payloads together form the backbone of the ```Unified Actions API```. While ```Actions``` facilitate operations on the Database, ```Payloads``` are used as specifications to specify data, selectors & configurations. 

- **Creating Databases**
  
    Action - <kbd>create-database</kbd> - Used for creating Databases.
  
    Payloads:
    - <kbd>name</kbd> - Select Name for Database to be created.
    
    Mode - <kbd>POST</kbd>
    
    ```
    {
        "action" : "create-database",
        "payload": {
            "name" : <name of Database>
        }
    }
    ```
    
- **Deleting Databases**
  
    Action - <kbd>delete-database</kbd> - Used for deleting Databases.
  
    Payloads:
    - <kbd>targetDatabase</kbd> - Select Name of the Database to be deleted.
    - <kbd>migrateCollections</kbd> - A Flag field. Indicates if the Collections in the Target Database need to be migrated. Allowed values:
      * `true` if collections migration is required.
      * `false` if collections migration is not required.
    - <kbd>destinationDatabase</kbd> - If migrateCollections is `true` then, select name of the Database where collections from targetDatabase should be migrated to.
    - <kbd>ifCollectionExists</kbd> - Flag Field. Indicates action to be taken if collection with same name exists in destination collection while migration. Allowed Values
      * `rename` - If collection exists, the incoming collection from source database is renamed by the system with system.
      * `replace` - If collection exists, the destination collection is replaced by the incoming collection.
      * `skip` - If collection exists, the incoming collection is skipped while other collections are migrated.
    
    Mode - <kbd>POST</kbd>
    
    ```
    {
        "action" : "delete-database",
        "payload": {
            "targetDatabase" : <name of Source Database>,
            "migrateCollection" : < `true` / `false` >,
            "targetDatabase" : <name of Destination Database>,
            "targetDatabase" : < `replace` / `rename` / `skip` >
        }
    }
    ```    
    
- **Creating Collections**
  
    Action - <kbd>create-collection</kbd> - Used for creating Collections.
  
    Payloads:
    - <kbd>database</kbd> - Name of Database where Collection is to be created
    - <kbd>name</kbd> - Select Name of Collection to be created<br/>  
    - <kbd>schema</kbd> - The specification of the Structure of the Collection i.e. the Headers/Titles of data and its type<br/> 
      
    Mode - <kbd>POST</kbd>
    
    ```
    {
        "action" : "create-collection",
        "payload: {
            "database" : <name of Database>,
            "name": <name of Collection>,
            "schema": {
                "title" : "type/details",
                ...
                "title" : "type/details"
            }
        }
    }
    ```

- **Clone Collections**
  
    Action - <kbd>clone-collection</kbd> - Used for Copy Collection from one database to another without deleting the collection at source.
  
    Payloads:
    - <kbd>sourceDatabase</kbd> - Name of Database from where Collection is to be copied.
    - <kbd>collection</kbd> -  Name of Collection to be copied.  
    - <kbd>destinationDatabase</kbd> - Name of Database to where Collection is to be copied. 
    - <kbd>actionIfExists</kbd> - Flag field. Action to be taken if copied collection exists in destination database.
      * `rename` - If collection exists, the incoming collection from source database is renamed as specified by user.
      * `replace` - If collection exists, the destination collection is replaced by the incoming collection.
      * `abort` - If collection exists, the incoming collection is skipped while other collections are migrated.
    - <kbd>newName</kbd> - New Name for Collection to be renamed. 
      
    Mode - <kbd>POST</kbd>
    
    ```
    {
        "action" : "clone-collection",
        "payload: {
            "sourceDatabase" : <name of source Database>,
            "collection": <name of Collection to be copied>,
            "destinationDatabase": <name of Destination Database>,
            "actionIfExists" : < `replace` / `rename` / `abort` >,
            "newName: : <new name for collection>
        }
    }
    ```
    
    
- **Migrate Collections**
  
    Action - <kbd>migrate-collection</kbd> - Used for Migrating/Moving Collection from one database to another by deleting it at source.
  
    Payloads:
    - <kbd>sourceDatabase</kbd> - Name of Database from where Collection is to be moved.
    - <kbd>collection</kbd> - Name of Collection to be Migrated. If `migrate-all` is passed insted of specific Collection name, then the system migrates all collections in the source database. In case of `migrate-all` the next three fields are not required.
    - <kbd>destinationDatabase</kbd> - Name of Database to where Collection is to be migrated. 
    - <kbd>actionIfExists</kbd> - Flag field. Action to be taken if migrated collection exists in destination database.
      * `rename` - If collection exists, the incoming collection from source database is renamed as specified by user.
      * `replace` - If collection exists, the destination collection is replaced by the incoming collection.
      * `abort` - If collection exists, the incoming collection is skipped while other collections are migrated.
    - <kbd>newName</kbd> - New Name for Collection to be renamed. 
      
    Mode - <kbd>POST</kbd>
    
    ```
    {
        "action" : "migrate-collection",
        "payload: {
            "sourceDatabase" : <name of source Database>,
            "collection": < `migrate-all` to migrate all collections / name of specific Collection to be migrated>,
            "destinationDatabase": <name of Destination Database>,
            "actionIfExists" : < `replace` / `rename` / `abort` >,
            "newName: : <new name for collection>
        }
    }
    ```

- **Deleting Collections**
  
    Action - <kbd>delete-collection</kbd> - Used for deleting Collections.
  
    Payloads:
    - <kbd>database</kbd> - Name of source Database.
    - <kbd>collection</kbd> - Select Name of Collection to be deleted.
      
    Mode - <kbd>POST</kbd>
    
    ```
    {
        "action" : "delete-collection",
        "payload: {
            "database" : <name of Database>,
            "collection": <name of Collection>
        }
    }
    ```

- **Adding Data to Collection**
  
    Action - <kbd>add-data</kbd> - Used for Adding new data to a Collection.
  
    Payloads:
    - <kbd>database</kbd> - Name of Database where Collection is to be created
    - <kbd>collection</kbd> - Name of Collection where data is to be added
    - <kbd>data</kbd> - The data that needs to be added to a collection. Headers must match the Schema header, else it generates error.
        
    Mode - <kbd>POST</kbd>
        
    ```
    {
        "action" : "add-data",
        "payload: {
            "database" : <name of Database>,
            "collection": <name of Collection>,
            "data": {
                "title" : "value",
                ...
                "title" : "value"
            }
        }
    }
    ```
    
- **Updating Data in a Collection**
  
    Action - <kbd>update-data</kbd> - Used for Updating existing data in a Collection.
  
    Payloads:
    - <kbd>database</kbd> - Name of Database where Collection is to be created
    - <kbd>collection</kbd> - Name of Collection where data is to be added
    - <kbd>marker</kbd> - Marker is a special token that specifies a particular data. A marker-key is the field-name/title and marker-value corresponds to the specific data to be updated. The format of specifying a marker is "markey-key : markey-value" keeping the single-spaces intact.
    - <kbd>data</kbd> - The changes to be made i the data. 
            
    Mode - <kbd>POST</kbd>
    
    ```
    {
        "action" : "update-data",
        "payload: {
            "database" : <name of Database>,
            "collection": <name of Collection>,
            "marker": "key : value",
            "data": {
                "title" : "value",
                ...
                "title" : "value"
            }
        }
    }
    ```

- **Deleting Data from a Collection**
  
    Action - <kbd>delete-data</kbd> - Used for Deleting existing data in a Collection.
  
    Payloads:
    - <kbd>database</kbd> - Name of Database where Collection is to be created
    - <kbd>collection</kbd> - Name of Collection where data is to be added
    - <kbd>marker</kbd> - Marker is a special token that specifies a particular data. A marker-key is the field-name/title and marker-value corresponds to the specific data to be deleted. The format of specifying a marker is "markey-key : markey-value" keeping the single-spaces intact.
            
    Mode - <kbd>POST</kbd>
    
    ```
    {
        "action" : "delete-data",
        "payload: {
            "database" : <name of Database>,
            "collection": <name of Collection>,
            "marker": "key : value"
        }
    }
    ```
    
- **Viewing Data of a Collection**
  
    Action - <kbd>get-data</kbd> - Used for Fetching existing data from a Collection.
  
    Payloads:
    - <kbd>database</kbd> - Name of Database where Collection is to be created
    - <kbd>collection</kbd> - Name of Collection where data is to be added
            
    Mode - <kbd>GET</kbd>
    
    ```
    {
        "action" : "get-data",
        "payload: {
            "database" : <name of Database>,
            "collection": <name of Collection>
        }
    }
    ```

- **Viewing Particular Data of a Collection**
  
    Action - <kbd>view-data</kbd> - Used for Fetching particular data from a Collection.
  
    Payloads:
    - <kbd>database</kbd> - Name of Database where Collection is to be created
    - <kbd>collection</kbd> - Name of Collection where data is to be added
    - <kbd>marker</kbd> - Marker is a special token that specifies a particular data. 
            
    Mode - <kbd>GET</kbd>
    
    ```
    {
        "action" : "view-data",
        "payload: {
            "database" : <name of Database>,
            "collection": <name of Collection>,
            "marker": "key : value"
        }
    }
    ```

- **View the complete Cluster**
  
    Action - <kbd>get-cluster</kbd> - Used for viewing Databases.
  
    Payloads: None
                
    Mode - <kbd>GET</kbd>
    
    ```
    {
        "action" : "get-cluster",
        "payload" : {}
    }
    ```
    
- **View databases in the Cluster**
  
    Action - <kbd>get-database</kbd> - Used for viewing databases in the Cluster.
  
    Payloads:
    - <kbd>database</kbd> - Value can be 'all', to get all databases or Name of a particular Database to be viewed.
                
    Mode - <kbd>GET</kbd>
    
    ```
    {
        "action" : "get-database",
        "payload" : {
            "database" : "all" (or specific database name)
        }
    }
    ```
    
- **View Collections in the Database**
  
    Action - <kbd>get-collection</kbd> - Used for viewing collections in the Database.
  
    Payloads:
    - <kbd>database</kbd> - Name of database having 
    - <kbd>collection</kbd> - Value can be 'all', to get all databases or Name of a particular Database to be viewed.
                
    Mode - <kbd>GET</kbd>
    
    ```
    {
        "action" : "get-collection",
        "payload" : {
            "database" : <database name>,
            "collection" : "all" (or specific collection name),
        }
    }
    ```
    
 
     


Project by ***[Tanmoy Sen Gupta](https://www.tanmoysg.com)***
