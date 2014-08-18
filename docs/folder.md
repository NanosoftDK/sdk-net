# Folders
Case files can be assigned to folders in order to group them together. The folders available from the API corresponds to the folders seen in the Penneo WebApp.

## Creating a folder
A folder is simply identified by its title. The example below shows how to create a folder:

```csharp
// Create a new folder object
var myFolder = new Folder();

// Set the folder title
myFolder.Title = "New Folder";

// Persist the new object
myFolder.Persist();
```

## Manipulating folder contents
An empty folder is not much fun. The example below shows how to assign and unassign case files, and how to list the contents of a folder:

```csharp
// Assign a case file to a folder
myFolder.AddCaseFile(caseFile1);

// Get a list of all the case files in the folder
var caseFiles = myFolder.GetCaseFiles();
for (var caseFile in caseFiles) {
	Console.WriteLine(caseFile.Title);	
}

// Now, remove that same case file again
myFolder.RemoveCaseFile(caseFile);

```

## Retrieve existing folders
There is several ways to retrieve folders from Penneo. Available methods for retrieving folders are:

* __Query.Find<Folder>(int id)__
Find one specific folder by its ID.
* __Query.FindAll<Folder>__
Find all folders accessible by the authenticated user.
* __Query.FindBy<Folder>(Dictionary\<string, object\> criteria = null, Dictionary\<string, string\> orderBy = null, int? limit = null, int? offset = null)__
Find all folders matching _criteria_ ordered by _orderBy_. If _limit_ is set, only _limit_ results are returned. If _offset_ is set, the _offset_ first results are skipped.
Criteria can either be _title_ or _metaData_.
* __Query.FindOneBy<Folder>(Dictionary\<string, object\> criteria = null, Dictionary\<string, string\> orderBy = null)__
Same as _FindBy_ setting _limit_ = 1 and _offset_ = null

Below is a couple of examples:

```csharp
// Retrieve all folders
var myFolders = Query.FindAll<Folder>();

// Retrieve a specific folder (by id)
var myFolder = Query.Find<Folder>(14284);

// Retrieve all folders that contains the word "the" in their title and sort descending on folder title
var myDocuments = Query.FindBy<Folder>(
	criteria: new Dictionary<string, object>{ { "title", "the" } },
	orderBy: new Dictionary<string, string>(){ { "title", "desc" } }
);

// Retrieve folders from offset 10 until 110 ordered by title in ascending order
var myFolders = Query.FindBy<Folder>(	
	orderBy: new Dictionary<string, string>(){ {"title", "asc" } },
	limit: 10,
	offset: 100
);

```

## Deleting a folder
Folders can be deleted, even if the contain case files. This will only delete the folder and its mappings, __NOT__ the case files. To delete a folder do the following:

```csharp
// Delete a folder
myFolder.Delete();
```