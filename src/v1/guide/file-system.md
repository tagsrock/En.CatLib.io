---
title: File system
type: guide
order: 230
---

## File system

> This article is translated by machine

CatLib provides file system abstraction that allows an API to operate on multiple local (cloud storage) file storage systems.

The CatLib file system has been integrated with a simple driver for handling local file systems.

### Context

If there are no special instructions, the sample project in this article is based on the following context, using the local file driver by default:

``` csharp
var fileSystemManager = App.Instance.Make<IFileSystemManager>();
```

You can get the disk instance of the file system through the `Disk ()` method of `IFileSystemManager`.

``` csharp
var disk = fileSystemManager.Disk();
```

### Default disk

CatLib default disk will be positioned to `Env.AssetPath`. For more information, please refer to [CatLib Core / Environment](application.html # environment)

### File system API

#### **Read**

The byte stream of the file can be read by the `Read ()` method.

``` csharp
byte[] data = disk.Read("helloworld.txt");
```

#### **Exists**

You can tell if the file exists by `Exists ()`.

``` csharp
bool isExists = disk.Exists("helloworld.txt");
```

#### **Delete**

You can delete a file with `Delete ()`.

``` csharp
disk.Delete("helloworld.txt");
```

#### **Write**

By `Write ()` can write a file, if the file already exists then overwrite the data.

``` csharp
disk.Wrire("helloworld.txt" , Encoding.Default.GetBytes("hello world"));
```

#### **Move**

Move the file or folder to the specified path.

`Move ()` can be used for renaming.

``` csharp
disk.Move("helloworld.txt" , "NewDir");
```

#### **Copy**

Copy the file or folder to the specified path.

``` csharp
disk.Copy("helloworld.txt" , "new-helloworld.txt");
```

#### **MakeDir**

Create a new `folder '.

``` csharp
disk.MakeDir("NewDir");
```

#### **GetAttributes**

Gets the properties of a file or folder.

``` csharp
disk.GetAttributes("helloworld.txt");
```

#### **GetSize**

Get the size of the file or folder (or the total size if it is a folder).

``` csharp
disk.GetSize("helloworld.txt");
```

#### **GetHandler**

Get a handle to a file or folder (you can manipulate the file or folder directly by hand)

``` csharp
var fileHandler = disk.GetHandler<IFile>("helloworld.txt");
```

``` csharp
var dirHandler = disk.GetHandler<IDirectory>("NewDir");
```

#### **GetList**

With `GetList ()`, you can get all the folders and file handles under the specified path (note that the subfolders do not iterate).

``` csharp
var list = disk.GetList();
```

### Customize the file system

The CatLib file system allows you to customize your file system drivers, and you only need to use `Extend ()` to customize your extensions.

``` csharp
fileSystemManager.Extend(()=>
{
    return new FileSystem(new Local("/user/data"));
},"MyDisk");
```

After extension, you can get extended instances with `Disk ()`.

``` csharp
var newDisk = fileSystemManager.Disk("MyDisk");
```