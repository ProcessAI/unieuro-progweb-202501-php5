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
### Criando a conexão:
```php
<?php
$conn = pg_connect("host=localhost dbname=meubanco user=meuusuario password=minhasenha");
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


