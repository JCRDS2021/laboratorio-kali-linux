# laboratorio-kali-linux
Conclusões sobre o laboratório de Kali Linux

ESTUDO PRÁTICO: ATAQUES DE FORÇA BRUTA COM MEDUSA

OBJETIVO DO ESTUDO

Este laboratório teve como propósito compreender, na prática, como ataques de força bruta funcionam, quais condições os tornam eficazes, e — sobretudo — o que pode ser feito para neutralizá-los. O ambiente foi deliberadamente vulnerável para permitir a observação do comportamento das ferramentas sem impacto em sistemas reais. 

AMBIENTE:
- VirtualBox - Rede Host Only
- Kali Linux + Metasploitable 2

FERRAMENTA PRINCIPAL:
- Medusa (brute-force)
- DVWA, Enum4linux

CENÁRIOS EXPLORADOS
- FTP - HTTP/Web Form
- SMB/Password Spraying

WORDLISTS UTILIZADAS
- Listas Simples customizadas
- rockyou.txt (referência)

COMANDOS CENTRAIS ESTUDADOS
medusa -h 192.168.56.101 -u admin -P wordlist.txt -M ftp
medusa -h 192.168.56.101 -U users.txt -p Senha@123 -M smbnt
enum4linux -U 192.168.56.101

REFLEXÕES E APRENDIZADOS

01 · O ataque começa antes da senha. Enumerar usuários válidos (via enum4linux ou respostas HTTP distintas) é muitas vezes o passo mais decisivo. Sem essa informação, a força bruta é ineficiente; com ela, o espaço de busca colapsa drasticamente. 

02 · Password spraying é cirúrgico. Ao tentar uma única senha comum em muitos usuários, evita-se bloqueios por tentativas repetidas no mesmo alvo. Isso tornou o ataque SMB mais realista e me fez entender por que políticas de lockout precisam ser acompanhadas de detecção por volume de IPs. 

03 · A defesa é camadas, não uma barreira. Rate limiting no FTP, lockout de conta no AD, WAF bloqueando tentativas em formulários web — nenhuma medida isolada é suficiente. O ambiente vulnerável deixou claro como a ausência de qualquer uma dessas camadas cria uma superfície facilmente explorável. 

04 · Wordlists contam a história do mundo real. Testar com rockyou.txt, mesmo que parcialmente, foi um lembrete de que as senhas que as pessoas escolhem são previsíveis. A melhor defesa continua sendo tornar a adivinhação irrelevante — com MFA, senhas longas e aleatórias, e gestores de senhas. 

RECOMENDAÇÕES DE MITIGAÇÃO
Desabilitar serviços desnecessários (FTP/SMBv1), implementar autenticação multifator, configurar fail2ban para bloqueio automático de IPs com múltiplas falhas, monitorar logs de autenticação com alertas para padrões anômalos, e adotar políticas de senha com entropia mínima verificada — não apenas comprimento. 
