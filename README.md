# Curso-DIO---Ciberseguran-a-do-Zero-a-pr-tica
Curso: Santander DIO - Cibersegurança do Zero a prática
DESAFIO Nº1
Simulando um ataque de BRUT FORCE de senhas com o Software MEDUSA utilizando a Distro KALI LINUX.
O desafio consiste em: 
Implementar, documentar e compartilhar um projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.
•	Configurar o ambiente: duas VMs (Kali Linux e Metasploitable 2) no VirtualBox, com rede interna (host-only).
•	Executar ataques simulados: força bruta em FTP, automação de tentativas em formulário web (DVWA) e password spraying em SMB com enumeração de usuários.
•	Documentar os testes: wordlists simples, comandos utilizados, validação de acessos e recomendações de mitigação.
Execução:
Baixar e instalar o Software Virtual Box.
Baixar e instalar a Distro Kali Linux
Baixar e instalar o VDK (Disco Virtual do Virtual Box) do Metasploitable 2
Configurar as placas de redes das Máquinas Virtuais como host-only para que as mesmas estejam dentro do mesmo range de IP.
Dentro do Kali Linux utilizando o NMAP (nmap -v 192.168.56.102/24) fazer uma varredura para descobrir o IP do alvo (Metasploitable 2)
resultado exibe o IP do ALVO e as portas com seus respectivos serviços:
O próximo passo é verificar as portas de interesse e seus respectivos serviços e versões dos mesmos no servidor alvo.
Usando o nmap com os seguintes parâmetros:
Nmap -sV -p 21,22,80,455,139 192.168.56.102 Obtemos o seguinte resultado:
Para realizar um ataque de Brut Force será necessário que sejam criadas duas World List, uma contendo os possíveis usuários e outra as possíveis senhas. Esses arquivos serão utilizados pelo Hydra para realizar o ataque.
O ataque será feito via medusa com os seguintes parâmetros:
Medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6
Os parâmetros apresentados nesse comando são:
-h (corresponde ao alvo) -U (arquivo contendo a world list com os possíveis usuários) -P (arquivo contendo a world list com as possíveis senhas) -M (indica o serviço a ser atacado) -t (é o número de threads simultâneas)
Com o SUCESSO do ataque agora devera ser feita a validação do acesso manualmente com o usuário e a senha encontrados.
Com o SUCESSO da conexão utilizando os dados encontrados pelo ataque realizado é finalizado o primeiro exercício.
O Segundo exercício será de: Automação de tentativas em formulário web (DVWA) .
Usaremos também como no exercício anterior duas world list (os arquivos users.txt e pass.txt), juntamente com os parâmetros encontrados no formulário da página de login como exibido na imagem acima.
O comando utilizado será:
Medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \ -m PAGE:’ /dvwa/login.php’ \ -m FORM:’username=^USER^&password=^PASS^&login=login’ \ -m ‘FAIL=login failed’ -t 6
                PROBLEMA!!!!!
Infelizmente nessa parte não consigo atestar a veracidade do que foi transmitido pois o Comando retorna TODAS as variáveis como SUCCESS! A cada vez que o mesmo é executado traz varias combinações e com o mesmo resultado. No próprio vídeo diferente do que é narrado pela pessoa que está ministrando o módulo, aparecem sempre desde a primeira vez em que o comando é executado SUCCESS para todos os resultados exibidos. A pessoa diz que Apartir do primeiro êxito, ou seja, do PRIMEIRO RESULTADO com SUCCESS todos os outros repetem o resultado SUCCESS! Não é verídica essa afirmação pois no vídeo TODOS os RESULTADOS desde que o comando é executado os resultados são IDENTICOS!
Diante do exposto fica impossível continuar essa parte do desafio, uma vez que CARTEAR um resultado como foi feito no vídeo de instrução não passa conhecimento algum!
O terceiro exercício será de: Utilização de password spraying em SMB com enumeração de usuários.
Vamos em primeiro lugar utilizar o enum4linux para aplicar todas as técnicas possíveis de Enumeração:
enum4linux -a 192.168.56.102 | tee enum4_output.txt
o parâmetro -a aplica todas as técnicas possíveis de Enumeração
o parâmetro tee direciona a saída do comando para o arquivo de texto enum4_output.txt
Apartir daí será criada a World List de Usuários e a World List de possíveis senhas (senha comumente usadas, senhas provenientes de vazamentos...)
A criação se dará da mesma forma feita no exercício anterior:
Criadas as World List vamos executar o comando do Medusa para o password spraying em SMB:
Medusa -h 192.168.56.102 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
Agora o parâmetro -M se refere ao ataque via SMB -t 2 são duas threads sumltâneas e -T são em até 50 Hosts
Podemos ver que obtivemos êxito ao encontrar uma combinação que nos fornecerá o acesso ao Usuário: msfadmin
O próximo passo verificar e validar o acesso com as credenciais encontradas:
Agora que fizemos a validação do acesso com os dados fornecidos de usuário e senha temos acesso ao ALVO.
