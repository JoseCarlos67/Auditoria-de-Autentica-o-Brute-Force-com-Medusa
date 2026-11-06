# Auditoria de Autenticação: Brute Force com Medusa

## 📌 Sobre o projeto

Este projeto foi desenvolvido como parte de um desafio prático da [Digital Innovation One (DIO)](https://www.dio.me/), com o objetivo de compreender e simular ataques de força bruta contra serviços de rede e aplicações vulneráveis, utilizando a ferramenta **Medusa**.

A proposta consiste em montar um laboratório controlado, identificar serviços expostos, realizar testes de autenticação e analisar medidas que podem ser adotadas para reduzir os riscos associados a ataques de força bruta e password spraying.

Diferentemente do ambiente sugerido no desafio, que utiliza Kali Linux e VirtualBox, optei por aproveitar meu ambiente Linux já existente, utilizando **Fedora como máquina de auditoria e Metasploitable 2 virtualizada com KVM/QEMU**.

Essa adaptação também permitiu praticar conceitos de administração Linux e virtualização.

## 🎯 Objetivos

- Compreender o funcionamento de ataques de força bruta.
- Conhecer e utilizar o Medusa para auditoria de autenticação.
- Identificar serviços de rede e suas possíveis vulnerabilidades.
- Praticar reconhecimento de serviços com Nmap.
- Compreender os riscos de credenciais fracas e configurações inseguras.
- Identificar medidas de prevenção e mitigação.
- Documentar procedimentos técnicos e evidências de um laboratório de segurança.

## 🖥️ Ambiente de laboratório

O laboratório foi construído utilizando duas máquinas:

| Máquina | Sistema / Tecnologia | Função |
|---|---|---|
| Máquina de auditoria | Fedora Linux | Execução das ferramentas de análise e auditoria |
| Máquina vulnerável | Metasploitable 2 | Alvo dos testes de segurança |
| Virtualização | KVM/QEMU | Hospedagem da máquina virtual vulnerável |

### Arquitetura

A máquina Fedora é utilizada diretamente no computador físico, enquanto a Metasploitable 2 é executada como máquina virtual por meio do KVM/QEMU.

```text
                  LABORATÓRIO LOCAL
                        
              ┌──────────────────────┐
              │    Fedora Linux      │
              │                      │
              │  Ferramentas:        │
              │  - Medusa            │
              │  - Nmap              │
              │                      │
              │  Máquina de auditoria│
              └──────────┬───────────┘
                         │
                    Rede virtual
                         │
              ┌──────────▼───────────┐
              │    KVM / QEMU        │
              │                      │
              │   Metasploitable 2   │
              │                      │
              │   Serviços vulneráveis│
              └──────────────────────┘
```

> **Observação:** A topologia e o modo de conexão da rede virtual devem ser considerados na configuração do laboratório. Os testes devem permanecer restritos ao ambiente controlado.

### Ferramentas utilizadas

- **Fedora Linux:** sistema operacional utilizado como estação de auditoria.
- **KVM/QEMU:** tecnologias responsáveis pela virtualização da máquina vulnerável.
- **Metasploitable 2:** máquina virtual intencionalmente vulnerável, utilizada como alvo.
- **Medusa:** ferramenta de auditoria de autenticação que permite testar combinações de usuários e senhas em diferentes serviços.
- **Nmap:** ferramenta de reconhecimento e identificação de serviços de rede.

## 🔎 Etapas do laboratório

### 1. Preparação do ambiente

Inicialmente, foi utilizado o Fedora já instalado no computador, dispensando a necessidade de instalar uma segunda máquina virtual com Kali Linux.

Em seguida, a Metasploitable 2 foi configurada como máquina virtual utilizando KVM/QEMU.

Essa abordagem permitiu aproveitar os recursos do próprio sistema operacional e praticar conceitos de virtualização sem depender do VirtualBox.

### 2. Reconhecimento da máquina vulnerável

Antes de executar os testes de autenticação, é necessário identificar os serviços disponíveis na máquina de destino.

Para isso, foi utilizado o Nmap.

Exemplo de reconhecimento:

```bash
nmap -sV <IP_DA_METASPLOITABLE>
```

O parâmetro `-sV` permite identificar versões dos serviços encontrados.

Essa etapa é importante porque possibilita compreender a superfície de exposição da máquina e identificar quais serviços poderão ser avaliados.

### 3. Auditoria de autenticação com Medusa

O Medusa foi utilizado para compreender como ferramentas automatizadas podem testar diferentes combinações de usuários e senhas contra serviços de autenticação.

O laboratório permite explorar os seguintes cenários propostos pelo desafio:

#### 3.1. FTP

O FTP é um protocolo utilizado para transferência de arquivos.

O objetivo do teste é avaliar a resistência do mecanismo de autenticação diante de tentativas automatizadas de acesso.

Aspectos observados:
- Resposta do serviço às tentativas de autenticação.
- Existência de credenciais fracas.
- Possibilidade de acesso não autorizado.

#### 3.2. Formulário de autenticação Web

O segundo cenário envolve a avaliação de um formulário de login web.

A proposta é compreender como ferramentas de auditoria automatizam tentativas de autenticação e quais características da aplicação podem dificultar ou facilitar esse processo.

Aspectos observados:
- Comportamento do formulário diante de credenciais incorretas.
- Respostas HTTP.
- Tratamento de erros de autenticação.
- Existência de mecanismos de proteção contra tentativas repetidas.

#### 3.3. SMB e Password Spraying

O SMB é um protocolo utilizado para compartilhamento de arquivos e recursos em redes.

Nesse cenário, o objetivo é compreender o conceito de password spraying.

Diferentemente da força bruta tradicional, que testa diversas senhas contra uma conta, o password spraying utiliza uma ou poucas senhas contra diferentes contas de usuário.

Aspectos observados:
- Identificação de usuários disponíveis.
- Comportamento do serviço SMB.
- Riscos associados à reutilização de senhas fracas.
- Possibilidade de acesso indevido a recursos compartilhados.

> **Nota:** Os cenários acima representam o escopo de testes do desafio. Os resultados individuais, comandos efetivamente executados e evidências devem ser registrados conforme a execução de cada etapa.

## 🛡️ Recomendações de segurança

Os testes permitem identificar algumas medidas importantes para reduzir os riscos de ataques de força bruta e password spraying.

| Vulnerabilidade / Risco | Medida de mitigação |
|---|---|
| Senhas fracas | Implementar políticas de senhas fortes |
| Tentativas excessivas de autenticação | Aplicar limitação de tentativas (rate limiting) |
| Password spraying | Monitorar tentativas distribuídas entre diferentes contas |
| Credenciais padrão | Alterar credenciais padrão durante a implantação |
| FTP sem criptografia | Substituir por SFTP ou FTPS |
| Ausência de monitoramento | Implementar logs e alertas de autenticação |
| Contas sem proteção adicional | Implementar autenticação multifator (MFA) quando aplicável |

Também é importante manter os serviços atualizados, restringir o acesso por firewall e desabilitar serviços desnecessários.

## 📚 Aprendizados

Durante a construção deste laboratório, os principais conhecimentos trabalhados foram:

- Configuração e utilização de máquinas virtuais com KVM/QEMU.
- Utilização do Fedora como ambiente de auditoria.
- Reconhecimento de serviços de rede com Nmap.
- Funcionamento do Medusa e sua utilização em auditorias de autenticação.
- Diferenças entre ataques de força bruta e password spraying.
- Identificação de riscos relacionados a mecanismos de autenticação.
- Importância de políticas de senhas, monitoramento e controle de acesso.

Outro ponto importante foi perceber que não é necessário utilizar exclusivamente uma distribuição voltada à segurança ofensiva, como Kali Linux, para realizar estudos de segurança. Um ambiente Linux convencional também pode ser preparado para executar ferramentas específicas de auditoria.

## ⚠️ Aviso de responsabilidade

Este laboratório foi desenvolvido exclusivamente para fins educacionais e de aprendizado em segurança da informação.

A Metasploitable 2 é intencionalmente vulnerável e não deve ser disponibilizada em redes públicas ou ambientes de produção.

Todas as atividades de reconhecimento e auditoria devem ser realizadas somente em sistemas próprios ou mediante autorização expressa do responsável pelo ambiente.

## 📖 Referências

- [Medusa – Documentação oficial](http://www.foofus.net/jmk/medusa/medusa.html)
- [Nmap – Documentação oficial](https://nmap.org/book/)
- [Metasploitable 2 – Rapid7](https://docs.rapid7.com/metasploit/metasploitable-2/)
- [KVM – Documentação do Linux](https://www.linux-kvm.org/)
- [QEMU – Documentação oficial](https://www.qemu.org/docs/master/)
- [Fedora Documentation](https://docs.fedoraproject.org/)

---

**Desafio:** Digital Innovation One (DIO)  
**Tema:** Brute Force com Medusa e Kali Linux  
**Adaptação:** Fedora Linux + KVM/QEMU + Metasploitable 2
