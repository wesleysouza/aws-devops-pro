# Módulo 2: Automação de infraestrutura

```
O CloudFormation permite que você crie e gerencia implantações de infraestrutura de AWS repetidamente e com previsibilidade.
```

Automação da infraestrutura é a possibilidade de definir a sua infraestrutura (ambiente operacional) por meio de código, como arquivos de configuração.

A infraestrutura como código (IaC) consiste em:
- Shell scripts;
- Código do aplicativo;
- AWS CloudFormation;
- Ferramentas de terceiros.

Por que automatizar?
- Reduzir erros humanos (evita desvios dos padrões de configuração);
- Tempos de resposta e lançamentos mais rápidos;
- Mantém a conformidade com a política como código:
	- Pode ser rastreado, validado e reconfigurado (tudo por meio de automação);

Benefícios:
- `Ter o controle de versão` e `ser gerenciada` exatamente como o código-fonte do aplicativo;
- `Ser criada, desativada` e `recriada` de modo repetido e confiável;
- Criada sob medida para testar a versão mais recente do meu aplicativo;

A IaC possibilita:
- A criação de vários ambientes;
- A criação de um ambiente idêntico para vários clientes.

AWS CloudFormation e DevOps
- `Criação automatizada de instâncias de pilhas` com modelos usando o console de gerenciamento da AWS, a AWS CLI, o AWS SDK ou o AWS CodePipeline;
- `Validação de parâmetros` para eliminar erros;
- `Encadeia e aninha pilhas` para construir sistemas complexos;
- `Recursos personalizados` que podem ser usados para conectar sistemas e validar implantações;
- `Recursos adicionais` que simplificam as mudanças na infraestrutura e ajudam a proteger os recursos críticos.

## 2.1 Estrutura do modelo do AWS CloudFormation: formato YAML

```yaml

AWSTemplateFormatVersion: "2020-01-09"

Description: #String com a descrição do template
	String
Paramethers: #Entradas do modelo
	set of parameters
Mappings: # Variáveis estáticas; pares chave-valor
	set of mappings
Conditions: #controles para se e quando determinados recursos são criados ou atualizados
	set of conditions
Transform: #Especifica a versão do AWS SAM a ser usada
	set of transforms
Resources: #[OBRIGATÓRIO]: Ativos da AWS a serem criados
	set of resources
Outputs: # Valores de recursos personalizados criados pelo modelo (URLs, nomes de usuário etc.)
	set of outputs

```

Observações:
- Os campos `parameters` e `conditions` possibilitam maios flexibilidade na utilização do modelo.
- `transform`: macro que possibilita ao Cloudformation alterar os inputs
- Apenas o `resources` é obrigatório.
- O `outputs` tem dois papéis fundamentais:
	- Saída da stack;
	- Capacidade de deixar variáveis ou valores expostos para outro template do CloudFormation que referencia minha pilha. Possibilitando a criação de pilhas encadeadas.

### 2.1.1 Modelos

Um modelo do CloudFormation é um arquivo JSON ou YAML que pode ser utilizado como blueprint para a criação de recursos na AWS. Por exemplo, em um modelo, é possível descrever uma instância do Amazon EC2, como o tipo de instância, o ID de AMI, os mapeamentos de dispositivo de bloco e o nome do par de chaves do Amazon EC2.

Por exemplo, se você criou uma pilha com o modelo a seguir, o CloudFormation provisiona uma instância com um ID de AMI **ami-0ff8a91507f77f867**, **t2.micro** como tipo de instância, **testkey** como nome de par de chaves e um volume Amazon EBS.

JSON
```json
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "A sample template",
    "Resources": {
        "MyEC2Instance": {
            "Type": "AWS::EC2::Instance",
            "Properties": {
                "ImageId": "ami-0ff8a91507f77f867",
                "InstanceType": "t2.micro",
                "KeyName": "testkey",
                "BlockDeviceMappings": [
                    {
                        "DeviceName": "/dev/sdm",
                        "Ebs": {
                            "VolumeType": "io1",
                            "Iops": 200,
                            "DeleteOnTermination": false,
                            "VolumeSize": 20
                        }
                    }
                ]
            }
        }
    }
}
```

YAML
```yaml
AWSTemplateFormatVersion: 2010-09-09
Description: A sample template
Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: ami-0ff8a91507f77f867
      InstanceType: t2.micro
      KeyName: testkey
      BlockDeviceMappings:
        - DeviceName: /dev/sdm
          Ebs:
            VolumeType: io1
            Iops: 200
            DeleteOnTermination: false
            VolumeSize: 20
```

Os modelos são bastante flexíveis, para saber mais consulte [Anatomia do Modelo](https://docs.aws.amazon.com/pt_br/AWSCloudFormation/latest/UserGuide/template-anatomy.html).


