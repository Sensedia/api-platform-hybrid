
<!-- TOC -->

# Validação do fluxo federado

Para realizar a validação do fluxo utilizamos uma API simples com um interceptor de mock e logs para caso necessário realizarmos uma análise.

Para isso é necessário ter concluído todo provisionamento e configuração do enviroment federado

* Acesse API Design > API Catalog
* Clique no **+**  para criar uma nova API

![Create Api](../images/create_api.jpg)

*  Preencha os campos conforme exemplo:
   * API Name
   * API version 
   * Base path
   * Selecione o Enviroment federado 
   * Clique em **SAVE AND NEXT**
 
![Create Api1](../images/create_api1.jpg)

*  Adicione um Resource:

![Create Api2](../images/create_api2.jpg)

*  Preencha os campos conforme exemplo:
   * Resource name
   * Clique em **SAVE** 


![Create Api3](../images/create_api3.jpg)

*  Preencha os campos conforme exemplo:
   * Escolha o metodo GET
   * Defina o Path
   * Clique em **SAVE OPERATION** 
   * Clique em **SAVE AND NEXT**



![Create Api4](../images/create_api4.jpg)


*  Preencha os campos conforme exemplo:
   * Selecione o Resource criado e a opreção GET
   * Adicione um Interceptor de Mock 


![Create Api5](../images/create_api5.jpg)

*  Preencha os campos conforme exemplo:
   * Adicione em body o exemplo abaixo
   * Defina o status como **200** 

```bash
{
    "firstName": "Paulo",
    "lastName": "Silva",
    "age": 25,
    "address":
    {
        "streetAddress": "Avenida das Palmeiras",
        "city": "São Paulo",
        "state": "SP",
        "postalCode": "1024"
    },
    "phoneNumber":
    [
        {
          "type": "Fixo",
          "number": "11 0000-1111"
        },
        {
          "type": "Celular",
          "number": "11 1.1111-0000"
        }
    ]
}
```

* Acesse a aba de interceptos de **Tracing**
   * Adicione dois interceptors de Log sendo um antes e um depois do Mock
   * Clique em **SAVE AND NEXT**
     
![Create Api6](../images/create_api6.jpg)


* Realize o Deploy da API
  
![Create Api7](../images/create_api7.jpg)

* Acesse API Design > API Catalog
   * Abra a API criada
   * Em Environments clique no Icone conforme a seta e copie o valor
   * Verifique o valor do Path
     
![Create Api8](../images/create_api8.jpg)

* Para realizar a validação, acesse um terminal ou ferramenta de requisição 
e execute um curl conforme exemplo 

```bash
curl -X GET https://api-support-aws.sensedia.com/mock-hybrid/mock
```

![Create Api9](../images/create_api9.jpg)

