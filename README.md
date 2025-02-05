!Modo Exec privilegiado 
enable
    !Configuração da Data e Hora
    clock set 19:30:00 04 February 2025
    !Modo de configuração Global
    conigure terminal
        !Configuração do Hostname
        hostname sw-01
        !Serviço de senhas
        service password-encryption
        !Serrviço de Marcação de data e hora
        service timestamps log datetime msec 
        !Desativar Resolução de Nomes
        no ip domain-lookup
        !Banner da Mensagem do Dia
        banner motd #AVISO: CUIDADO#
        !Segurança do Modo Enable
        enable secret 123@senac
        !Criando usuários e senhas de acesso
        username senac secret 123@senac
        username mauricio password 123@senac
        username admin privilege 15 secret 123@senac 
        !Configurar a linha (Line) Console
        Line Console 0
            !Forçar Login Local
            login local
            !Forçar senha no console (Redundancia)
            password 123@senac
            !Sincronismo de Log
            logging synchronous 
            !Tempo de Inatividade
            exec-timeout 5 30 
            !Sair de todos os Modos
            end
    !Salvar as Configurações da RAM para a NVRAM
    copy running-config startup-config
!Para sair do modo Enabe
disable

enable
    !Visualizando as Configurações
    show running-config 

    enable
        configure terminal
        !configurar o Gateway Padrão
        ip default-gateway 192.168.1.254
        !Configurar a SVI do Switch
        interface vlan 1
            !Configurar a Descrição
            description Interface de SVI
            !Configurar o Endereço IPv4
            ip address 192.168.1.250 255.255.255.0
            !Iniciar a Interface SVI
            no shutdown
            end
    write

    show running-config
    show ip interface brief
    show vlan brief


    enable
    configure terminal
    !Configurar o Dominio FQDN
    !hostame.domain.br
    ip domain-name senac.br
    !Habilitar o SSH Server
    crypto key generate rsa general-keys modulus 1024
    !Habilitar a Versão 2
    ip ssh version 2
    !Tempo de Inatividade
    ip ssh time-out 60
    !Numero Maximo de Conexões
    ip ssh authentication-retries 2
    end
write


ssh -l senac 192.168.1.250
ssh -l admin 192.168.1.251


