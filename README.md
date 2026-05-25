# Sistema de Laudos ECG — Clínica ENCOR

Sistema web para geração de laudos de eletrocardiograma com folha timbrada, biblioteca de 45+ conclusões padronizadas, banco de pacientes local e integração com VIDaaS para assinatura digital ICP-Brasil.

## Acesso online

🔗 **[Abrir sistema](https://SEU-USUARIO.github.io/laudos-ecg/)**

> Substitua `SEU-USUARIO` pelo seu nome de usuário do GitHub após publicar.

## Funcionalidades

- **Editor com 3 abas**: Identificação, Medidas, Conclusão
- **45+ conclusões padronizadas** com modo aditivo (combine múltiplos diagnósticos)
- **Cálculos automáticos**: QTc Bazett/Fridericia, eixo elétrico, alertas de QT alterado
- **Folha timbrada ENCOR** embutida com marca d'água e assinatura escaneada
- **Banco de pacientes local** (Ctrl+L) com busca e exportação JSON
- **Salvamento automático em pasta** com nome do paciente (Chrome/Edge)
- **Compatível com VIDaaS** para assinatura digital ICP-Brasil qualificada

## Como usar

1. Acesse a URL do sistema
2. Configure uma pasta padrão de salvamento (botão 📁 Pasta destino) — apenas uma vez
3. Preencha o laudo nas 3 abas
4. Clique em 💾 Salvar PDF (ou 🔏 Salvar PDF p/ VIDaaS)

## Tecnologia

- HTML/CSS/JavaScript puro (sem dependência de servidor)
- File System Access API para salvamento direto
- html2canvas + jsPDF para geração de PDFs
- LocalStorage para persistência de dados

## Privacidade

Todos os dados ficam armazenados **apenas no navegador do médico**. Nenhuma informação de paciente é enviada para servidores externos. O banco de pacientes está em LocalStorage, criptografado pelo próprio Chrome.

## Atalhos

| Tecla | Ação |
|---|---|
| Ctrl+S | Salvar paciente no banco |
| Ctrl+Shift+S | Salvar PDF na pasta |
| Ctrl+P | Imprimir |
| Ctrl+L | Abrir banco de pacientes |
| Ctrl+N | Novo laudo |

## Compatibilidade

- ✅ Chrome (recomendado — todas as funcionalidades)
- ✅ Microsoft Edge
- ✅ Brave
- ⚠ Firefox/Safari: funciona, mas sem salvamento automático em pasta

---

Desenvolvido para uso clínico privado da Clínica ENCOR — Barreiras-BA.
