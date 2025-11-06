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

## Criando listas  
### Colocando nomes e usuários comuns em diferentes arquivos
### Com o comando abaixo criamos um arquivo de nome **users.txt** e nele inserimos alguns nomes de usuários padrões: 
```echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt```
### Criamos também um arquivo de nome **pass.txt** e inserimos algumas senhas padrões, usando o seguinte código:
```echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt```

## Rodando o primeiro ataque usando o Medusa  
<img src="https://github.com/LipehSanttos/desafio-kali-dio/blob/main/images/snap03.png" alt ="Screenshot 3" width="650">
