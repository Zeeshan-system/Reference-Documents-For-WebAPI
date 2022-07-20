# Implement Mapster to convert object of model class to DTO

## Basic Conversion
* Implement the Model class (Instance below)

```csharp
public class EmployeeInfo
    {
        [Key]
        public int ID { get; set; }
        
        public string Name { get; set; }
        
        public int DeptID { get; set; }
        
        public DateTime DateJoined { get; set; }
        
    }
```
* Implement DTO class

```csharp
public class EmployeeINFODTO
    {
        public int ID { get; set; }
        
        public string Name { get; set; }
        
        public int DeptID { get; set; }
        
        
    }
```
* Use the Adapt method to convert from one type object to another
* ProjectToType can be used in Query while fetching data from EF

### Adapt Example (Mapping to new object):

```csharp
var destObject = sourceObject.Adapt<Destination>();

//Example from above Models
Employee emp = new Employee(){
    ID=1,
    Name="XYZ",
    DeptID=1,
    DateJoined=DateTime.Now;
};
var obj = emp.Adapt<EmployeeINFODTO>()
```
### Adapt Example (Mapping to existing objects):
```csharp
sourceObject.Adapt(destObject);
```

### ProjectToType Example:
```csharp
using (MyDbContext context = new MyDbContext())
{
    // Build a Select Expression from DTO
    var destinations = context.Sources.ProjectToType<Destination>().ToList();

    // Versus creating by hand:
    var destinations = context.Sources.Select(c => new Destination {
        Id = p.Id,
        Name = p.Name,
        Surname = p.Surname,
        ....
    })
    .ToList();
}
```