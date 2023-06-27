# AWS Lambda e Golang

## Uso básico

1. Inicie o módulo do Golang

```bash
go mod init <nome-da-pasta>
```

2. Baixe os pacote AWS Lambda para Golang

```bash
go get -u github.com/aws/aws-lambda-go/lambda
```

> **Aviso:** Para informações precisa, veja este [tutorial](https://docs.aws.amazon.com/pt_br/lambda/latest/dg/golang-handler.html) no portal da AWS em caso de mudanças no uso básico

3. Crie o arquivo de ponto inicial de Golang

```bash
touch main.go
```

4. Implemente um dos seguintes trechos de código no arquivo `main.go`

### Exemplo 1

```go
package main

import (
  "fmt"

  "github.com/aws/aws-lambda-go/events"
  "github.com/aws/aws-lambda-go/lambda"
)

func main() {
  lambda.Start(handler)
}

func handler(request events.APIGatewayProxyRequest) (events.APIGatewayProxyResponse, error) {
  fmt.Println("Hello λ!")

  response := events.APIGatewayProxyResponse{
    StatusCode: 200,
  }

  return response, nil
}
```

### Exemplo 2

```go
package main
import (
  "context"
  "fmt"

  "github.com/aws/aws-lambda-go/lambda"
)

type MyEvent struct {
  Name string `json:"name"`
}

func handler(ctx context.Context, name MyEvent) (string, error) {
  return fmt.Sprintf("Hello %s!", name.Name), nil
}

func main() {
  lambda.Start(handler)
}
```

5. Construa um arquivo binário do arquivo `main.go`

```bash
GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -o main main.go
```

> **Observação:** o arquivo binário será construído e nomeado como `main` e sem extensão do Golang **(.go)**

6. Comprima o arquivo binário `main`

```bash
zip <nome-desejado>.zip <arquivo-alvo>
```

> **Nota:** pode ser necessário instalar o pacote zip no Linux em caso de ausência em sua máquina

7. Faça upload deste arquivo no painel da função Lambda na AWS

<details>
  <summary>Captura de tela</summary>
  <img src="" />
</details>

8. Ainda no portal da AWS, vá nas configurações do runtime, **altere o nome do handler** pelo nome do arquivo binário

<details>
  <summary>Captura de tela</summary>
  <img src="" />
</details>

> **Obs:** por padrão, a AWS nomeia o handler do runtime como **hello**
