# Programação Web em PHP 5

## Introdução
- **Objetivo**: Apresentar conceitos fundamentais do PHP 5 para desenvolvimento web
- **Tópicos abordados**:
  - Variáveis GET e POST
  - Sessões
  - Conexão com PostgreSQL
  - Includes

## Variáveis GET e POST
### Definição:
- `$_GET`: Captura dados enviados via URL (query string)
- `$_POST`: Captura dados enviados via formulário

### Exemplo GET:
```php
<?php
echo "O nome enviado foi: " . $_GET['nome'];
?>
```

### Exemplo POST:
```php
<?php
echo "O nome enviado foi: " . $_POST['nome'];
?>
```

### Diferenças principais:
- GET expõe os dados na URL, POST não
- GET tem limite de tamanho, POST é mais flexível

## Sessões em PHP
### O que é uma sessão?
- Mecanismo para armazenar dados entre requisições

### Como iniciar uma sessão:
```php
<?php
session_start();
?>
```

### Armazenando valores na sessão:
```php
<?php
$_SESSION['usuario'] = "João";
?>
```

### Recuperando valores da sessão:
```php
<?php
echo $_SESSION['usuario'];
?>
```

### Finalizando uma sessão:
```php
<?php
session_destroy();
?>
```

## Conexão com PostgreSQL
### Instalação da extensão `pgsql`

No arquivo de configuração do Php5 (php.ini) inclua a linha abaixo para ativiar o PostgreSQL no Php5.

```
extension=php_pgsql.dll
```

### Criando o BD

No PostgreSQL crie uma banco de dados chamado "produtos" no PgAdmin.

Crie a tabela abaixo no PgAdmin:
```
CREATE TABLE IF NOT EXISTS public.usuario
(
    idusuario integer NOT NULL DEFAULT nextval('usuario_idusuario_seq'::regclass),
    username character varying(50) COLLATE pg_catalog."default" NOT NULL,
    password character varying(32) COLLATE pg_catalog."default" NOT NULL,
    status boolean DEFAULT true,
    CONSTRAINT usuario_pkey PRIMARY KEY (idusuario)
)
```

Insira no PgAdmin o registro abaixo:

```
INSERT INTO public.usuario(
	username, password, status)
	VALUES ("admin", "123456", true);

```


### Criando a conexão:
```php
<?php
$conn = pg_connect("host=localhost dbname=produtos user=postgres password=123456");
if (!$conn) {
    echo "Erro na conexão com o banco de dados.";
}
?>
```

### Executando uma consulta:
```php
<?php
$resultado = pg_query($conn, "SELECT * FROM usuarios");
while ($linha = pg_fetch_assoc($resultado)) {
    echo "Nome: " . $linha['nome'] . "<br>";
}
?>
```

### Inserindo dados:
```php
<?php
$query = "INSERT INTO usuarios (nome, email) VALUES ('Carlos', 'carlos@email.com')";
$resultado = pg_query($conn, $query);
if ($resultado) {
    echo "Registro inserido com sucesso!";
} else {
    echo "Erro ao inserir registro.";
}
?>
```

### Atualizando dados:
```php
<?php
$query = "UPDATE usuarios SET email = 'novoemail@email.com' WHERE nome = 'Carlos'";
$resultado = pg_query($conn, $query);
if ($resultado) {
    echo "Registro atualizado com sucesso!";
} else {
    echo "Erro ao atualizar registro.";
}
?>
```

### Fechando a conexão:
```php
<?php
pg_close($conn);
?>
```

## Includes
### O que são?
- Permitem reutilizar código em múltiplas páginas

### Exemplo de uso:
```php
<?php
include 'cabecalho.php';
?>
```

### Diferença entre `include` e `require`:
- `include`: Em caso de erro, exibe um aviso e continua
- `require`: Em caso de erro, interrompe a execução

## Criando a página de produtos

### Criando a tabela de produtos no BD

```
CREATE TABLE IF NOT EXISTS public.produto
(
    idproduto integer NOT NULL DEFAULT nextval('produto_idproduto_seq'::regclass),
    produtonome character varying(100) COLLATE pg_catalog."default" NOT NULL,
    produtopreco real NOT NULL DEFAULT 0,
    produtofoto character varying(150) COLLATE pg_catalog."default",
    produtostatus boolean DEFAULT false,
    CONSTRAINT produto_pkey PRIMARY KEY (idproduto)
)
```

Se precisar recriar a tabela, primeiro a remova com o SQL abaixo e depois recrie novamente com o SQL acima.
```
DROP TABLE IF EXISTS public.produto;
```

### Criando a GRID de produtos

Uma referência de exemplo:
```
<table border="1" align="center" width="100%">
            <tr>
                <th bgcolor="#CCCCCC"><input type="checkbox" name="todos"></th>
                <th bgcolor="#CCCCCC">idproduto</th>
                <th bgcolor="#CCCCCC">Nome</th>
                <th bgcolor="#CCCCCC">Preço</th>
                <th bgcolor="#CCCCCC">Status</th>
            </tr>
            <?php
            $resultado = pg_query($conn, "SELECT * FROM produto");
            while ($linha = pg_fetch_assoc($resultado)) {

                //print_r($linha);
            ?>
            <tr>
                <td><input type="checkbox" name="todos"></td>
                <td><?php echo $linha["idproduto"] ;?></td>
                <td><?php echo $linha["produtonome"] ;?></td>
                <td><?php echo $linha["produtopreco"] ;?></td>
                <td><?php
                if($linha["produtostatus"] == "t") echo "Ativo";
                else echo "Desativado"
                ?>
                </td>
            </tr>    
            <?php
            }
            ?>
        </table>
```



