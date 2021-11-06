![](https://raw.githubusercontent.com/SergeyMi37/appmsw-docbook/master/doc/Screenshot_1.png)
## appmsw-docbook

[![license](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An application for installation into your instance of the DoсBook database, which is no longer supplied with IRIS, but sometimes it is very necessary, at least as a local documentation.

## Installation with ZPM

If ZPM the current instance is not installed, then in one line you can install the latest version of ZPM.
```
set $namespace="%SYS", name="DefaultSSL" do:'##class(Security.SSLConfigs).Exists(name) ##class(Security.SSLConfigs).Create(name) set url="https://pm.community.intersystems.com/packages/zpm/latest/installer" Do ##class(%Net.URLParser).Parse(url,.comp) set ht = ##class(%Net.HttpRequest).%New(), ht.Server = comp("host"), ht.Port = 443, ht.Https=1, ht.SSLConfiguration=name, st=ht.Get(comp("path")) quit:'st $System.Status.GetErrorText(st) set xml=##class(%File).TempFilename("xml"), tFile = ##class(%Stream.FileBinary).%New(), tFile.Filename = xml do tFile.CopyFromAndSave(ht.HttpResponse.Data) do ht.%Close(), $system.OBJ.Load(xml,"ck") do ##class(%File).Delete(xml)
```
If ZPM is installed, then `appmsw-docbook` can be set with the command
```
zpm:USER>install appmsw-docbook
```
## Installation with Docker

## Prerequisites
Make sure you have [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [Docker desktop](https://www.docker.com/products/docker-desktop) installed.

## Installation 
Clone/git pull the repo into any local directory

```
$ git clone https://github.com/SergeyMi37/appmsw-docbook.git
```

Open the terminal in this directory and run:

```
$ docker-compose build

...
[appmsw-docbook]        Compile SUCCESS
[appmsw-docbook]        Activate START
[appmsw-docbook]        Configure START
Creating docbook4 db, namespace, resource and role with one command
Creating Database docbook4... done!
Creating Namespace docbook4... done!
Creating Interoperability mappings ... done!
Adding Interoperability SQL privileges ... done!
Creating CSP Application ... done!

Copy db from: /opt/irisapp/iris/mgr/docbook/iris.dat
        to: /usr/irissys/mgr/docbook4/IRIS.DAT
Copy web from: /opt/irisapp/iris/csp/docbook
       to: /usr/irissys/csp/docbook4
Load http://a645e6ca2401:52773/csp/docbook4/DocBook.UI.Page.cls
[appmsw-docbook]        Configure SUCCESS
[appmsw-docbook]        MakeDeployed START
...

```

3. Run the IRIS container with your project:

```
$ docker-compose up -d
```
# You can download the documentation at

http://localhost:52663/csp/docbook4/DocBook.UI.Page.cls

# If the name of the new area dock4 does not suit you, you can create a new one with a different name in the terminal with the command:
```
docker-compose exec iris iris session iris


USER>do ##class(appmsw.util.database).initdb("docbook18")

Creating docbook18 db, namespace, resource and role with one command
Creating Database docbook18... done!
Creating Namespace docbook18... done!
Creating Interoperability mappings ... done!
Adding Interoperability SQL privileges ... done!
Creating CSP Application ... done!

Copy db from: /irisdev/app/iris/mgr/docbook/iris.dat
        to: /usr/irissys/mgr/docbook18/IRIS.DAT
Copy web from: /irisdev/app/iris/csp/docbook
       to: /usr/irissys/csp/docbook18
Load http://78e95e7e0a0d:52773/csp/docbook18/DocBook.UI.Page.cls

```