# Guia Completo de Manipulação de Arquivos em Java

## 1. Introdução

Este guia aborda as principais técnicas de manipulação de arquivos em
Java, incluindo criação, leitura, escrita, deleção e boas práticas com
as bibliotecas `java.io` e `java.nio`, além de dicas para lidar com
arquivos grandes e codificação de texto.

------------------------------------------------------------------------

## 2. Estrutura do Guia

1.  Fundamentos de File e Streams\
2.  Criação de Arquivos\
3.  Leitura de Arquivos\
4.  Gravação em Arquivos\
5.  Deleção de Arquivos\
6.  Uso da API `Files` (`java.nio.file`)\
7.  Boas Práticas: Try-with-resources, bufferização, encoding\
8.  Leitura de Arquivos Grandes / Alto Desempenho\
9.  Verificação e atributos de arquivo\
10. Exemplo prático: ciclo completo

------------------------------------------------------------------------

## 3. Conteúdo Detalhado

### 3.1. Fundamentos: `File`, Streams e NIO

-   A classe `java.io.File` representa nomes e caminhos de
    arquivos/diretórios, com métodos como `createNewFile()`, `delete()`,
    `exists()`, `getName()`, `length()`, etc.\
-   Streams em Java:
    -   **Byte Streams**: `FileInputStream`, `FileOutputStream`,
        `BufferedInputStream`, `BufferedOutputStream` (dados binários).\
    -   **Character Streams**: `FileReader`, `FileWriter`,
        `BufferedReader`, `BufferedWriter` (texto).

### 3.2. Criar Arquivos

-   Usando `File`: `createNewFile()` retorna `true` se criou ou `false`
    se já existia.

### 3.3. Ler Arquivos

-   `BufferedReader + FileReader`: ler linha a linha é eficiente e
    claro.\
-   Outras opções:
    -   `Scanner` para leitura simplificada.\
    -   `Files.readAllLines()` para carregar todas as linhas numa
        lista.\
    -   `RandomAccessFile` para leitura com posições arbitrárias.

### 3.4. Escrever em Arquivos

-   `BufferedWriter + FileWriter`: permite escrever texto com
    eficiência.\
-   Alternativa: `FileWriter` sozinho (menos eficiente, mas simples).

### 3.5. Deletar Arquivos

-   Usando `File.delete()`, retorna `true` se deletou, ou `false` caso
    contrário.

### 3.6. Usar API `Files` (NIO)

-   Para operações simples: `Files.write()`, `Files.readAllBytes()`,
    `Files.readAllLines()`.\
-   Exemplos:
    -   `Files.write(Path, byte[], OpenOption...)`\
    -   `StandardOpenOptions`: `WRITE`, `APPEND`, `CREATE`,
        `CREATE_NEW`, etc.

### 3.7. Boas Práticas

-   **Try-with-resources**: fecha automaticamente recursos, evita
    vazamentos.\
-   **Bufferização**: melhorar desempenho com
    `BufferedReader/BufferedWriter`.\
-   **Tratamento de exceções**: sempre capturar `IOException` ou
    `FileNotFoundException`.\
-   **Codificação**: especifique encoding ao ler/gravar
    (`StandardCharsets.UTF_8`, etc.).

### 3.8. Arquivos Grandes / Alta Performance

-   `FileChannel` + `ByteBuffer` para leitura eficiente de arquivos
    volumosos.

### 3.9. Verificação e Atributos

-   Usar métodos como `exists()`, `canRead()`, `canWrite()`, `length()`,
    `getAbsolutePath()`.

### 3.10. Exemplo Prático (Criação → Escrita → Leitura → Deleção)

``` java
import java.io.*;
import java.nio.file.*;
import java.util.List;

public class FileCycle {
    public static void main(String[] args) {
        Path p = Paths.get("test.txt");

        // 1. Criar e Escrever
        try (BufferedWriter writer = Files.newBufferedWriter(p)) {
            writer.write("Linha 1\nLinha 2");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 2. Ler e Imprimir
        try (BufferedReader reader = Files.newBufferedReader(p)) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 3. Deletar
        try {
            if (Files.deleteIfExists(p)) {
                System.out.println("Arquivo deletado.");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

------------------------------------------------------------------------

## 4. Sumário Rápido (Tabela)

  ------------------------------------------------------------------------
  Operação     Recurso/Classe                Observações
  ------------ ----------------------------- -----------------------------
  Criar        `File.createNewFile()`        Checa existência

  Verificar    `exists()`, `canRead()`, etc. Informações antes de operar

  Ler texto    `BufferedReader`, `Scanner`,  Depende de caso de uso
               `Files`                       

  Escrever     `BufferedWriter`,             Use buffer para performance
  texto        `FileWriter`                  

  NIO avançado `Files`, `FileChannel`,       Leitura/gravação eficiente
               `Paths`                       

  Deletar      `File.delete()` ou            Simples
               `Files.deleteIfExists()`      

  Boas         Try-with-resources, encoding, Segurança e robustez
  práticas     tratamento de erros           
  ------------------------------------------------------------------------

------------------------------------------------------------------------

## 5. Referências Utilizadas

-   GeeksforGeeks --- [File Handling in
    Java](https://www.geeksforgeeks.org/java/file-handling-in-java/)\
-   W3Schools --- [Java
    Files](https://www.w3schools.com/java/java_files.asp)\
-   Oracle Docs --- [File
    I/O](https://docs.oracle.com/javase/tutorial/essential/io/file.html)\
-   Medium --- [File Handling in
    Java](https://medium.com/@liezelshaw_45834/file-handling-in-java-a-beginners-guide-1a54442a9c08)\
-   DigitalOcean --- [Java Read File Line by
    Line](https://www.digitalocean.com/community/tutorials/java-read-file-line-by-line)
