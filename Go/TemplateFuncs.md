# Template parsing must take place after adding FuncMap to the template in order to use it

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Hello World!</title>
</head>
<body>
    <h1>D1: {{uc .D1}}</h1>
    <h1>D2: {{.D2}}</h1>
    <ul>
        {{ range .D3 }}
        <li>{{.}}</li>
        {{end}}
    </ul>
    <ul>
    {{ range $key, $val := .D4 }}
    <li>{{uc $key}}: {{uc $val}}</li>
    {{ end }}
    </ul>
    <h1> And now the whole object of data again </h1>
    {{.}}
</body>
</html>
```

```go
// doens't work, will first parse and than add functions to the object.
// when trying to reference a function in the template (above on line 8) it will fail
// function uc is undefined cause it wasn't part of the render phase.
tpl = template.Must(template.ParseGlob("templates/*.gohtml")).Funcs(fm)
```

```go
// Working, it will parse the template when Funcs are already available thus static parsing
// will create the correct file.
tpl = template.Must(template.New("").Funcs(fm).ParseGlob("templates/*.gohtml"))
```