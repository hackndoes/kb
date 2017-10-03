# List dirctories in job workspace on master for dynamic parameter

```groovy
import groovy.io.FileType
def list = []
def workspace = new File(".")
workspace.eachFile (FileType. DIRECTORIES) { dir ->
  list << dir.path
}
return list
```