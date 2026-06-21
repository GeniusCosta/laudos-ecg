# Sistema de Laudos ECG — Clínica ENCOR

Gerador de laudos de eletrocardiograma (adulto e pediátrico) em página única, com pré-visualização A4 timbrada, assinatura, geração de PDF e funcionamento offline como aplicativo instalável (PWA).

**Versão atual:** `v.e4436555`
**Tema:** dark, alinhado ao editor de Ecocardiograma ENCOR.

---

## Novidades desta versão

- **Dropdown de ritmo simplificado:** Sinusal · Atrial · Juncional · Ventricular.
- **Motor de conclusão automático:** deriva as linhas da conclusão a partir do ritmo e das medidas (FC, iPR, QRS, eixo, morfologia de V1), reconciliando a cada alteração e preservando o texto manual.
- **Chip "Arritmia sinusal"** com o comentário de variante fisiológica.
- Mantidos os recursos anteriores: módulo pediátrico (Tabela 1 SBC), calculadoras de Sgarbossa e Brugada, comparação com laudo anterior, anexo do traçado ao PDF e PWA offline.

---

## Recursos

### Editor
- Três abas: Identificação · Medidas · Conclusão.
- Pré-visualização A4 em tempo real com timbre ENCOR e assinatura do Dr. Genius Costa.
- Geração de PDF (html2canvas + jsPDF), PDF com slot para VIDaaS, e impressão direta.
- Banco de pacientes (localStorage) com **autocomplete de nomes**, **filtros** (Todos/Adulto/Pediátrico) e **ordenação** (salvos, exame, nome).
- Atalhos: `Ctrl+S` salvar · `Ctrl+L` banco · `Ctrl+N` novo · `Ctrl+Shift+S` PDF · `Alt+1/2/3` abas · `Ctrl+←/→` aba anterior/próxima.

### Modo pediátrico
- Faixa etária automática (0–1 dia … 12–16 anos) a partir de idade + unidade, com override manual.
- Validação de FC, SÂQRS, iPR, QRS e QTc por faixa (III Diretriz SBC, Tabela 1).
- Em pediátrico, o motor de conclusão usa os limiares da faixa em vez dos valores de adulto.

### Motor de conclusão — regras
Conclusão padrão: **"Eletrocardiograma dentro dos limites da normalidade."** (restaurada quando não há achados).

| Condição | Conclusão gerada |
|---|---|
| Sinusal + FC < 50¹ | Bradicardia sinusal. |
| Sinusal + FC > 100¹ | Taquicardia sinusal. |
| Atrial + FC > 100¹ | Fibrilação atrial com resposta ventricular elevada. |
| Atrial + FC < 50¹ | Fibrilação atrial com baixa resposta ventricular. |
| Atrial + FC 50–100¹ | Fibrilação atrial com resposta ventricular controlada. |
| Juncional | Ritmo juncional. |
| Ventricular | Ritmo de origem ventricular. |
| iPR > 200 ms² | Bloqueio atrioventricular de primeiro grau. |
| QRS > 120 ms + R' em V1 | Bloqueio completo do ramo direito. |
| QRS > 120 ms + S/QS em V1 | Bloqueio completo do ramo esquerdo. |
| SÂQRS < −30°³ | Bloqueio divisional anterossuperior esquerdo. |

¹ Em modo pediátrico, os limites de FC vêm da faixa etária (Tabela 1 SBC).
² Em modo pediátrico, o limite de iPR vem da faixa etária.
³ Critério implementado como **desvio do eixo à esquerda** (eixo mais negativo que −30°). O consenso costuma usar −45°; ajustável.

> Achados múltiplos se acumulam. Remover a condição remove a linha correspondente. O motor não reorganiza a conclusão enquanto o usuário está digitando nela.

### Calculadoras clínicas
- **Sgarbossa** (IAM em vigência de BRE / ritmo de marca-passo), incluindo o critério modificado de Smith (ST/S ≤ −0,25). Sugerida automaticamente quando há BRE na conclusão.
- **Brugada** (taquicardia de QRS largo: TV × TSV com aberrância). Sugerida quando QRS ≥ 120 ms e FC ≥ 100.
- Cada resultado pode ser inserido na conclusão com um clique.

### Comparação com laudo anterior
- Botão **⇄ Comparar**: localiza o laudo anterior do mesmo paciente no banco e mostra lado a lado.
- Tabela de medidas com diferenças destacadas, diff da conclusão (novo / ausente / mantido) e **alertas de progressão** automáticos: progressão de bloqueio AV, fibrilação atrial nova, flutter novo, IAM novo, BRE/BRD novos, SVE evoluindo com strain.

### Anexo do traçado de ECG
- Botão **📎 Anexar ECG**: imagem (foto de celular, JPEG/PNG) ou PDF do traçado impresso.
- Cada anexo entra como página adicional no PDF do laudo (laudo autocontido). PDFs são rasterizados via pdf.js.
- Anexos ficam em memória (não no banco) e são limpos ao iniciar/abrir outro paciente.

### PWA / offline
- Instalável como aplicativo (Windows/macOS/Android) via botão **⬇️ Instalar app**.
- Service worker com cache do app e das bibliotecas; funciona 100% offline após a primeira carga online.

---

## Arquivos do repositório

| Arquivo | Função |
|---|---|
| `index.html` | Aplicativo completo (HTML/CSS/JS em arquivo único). |
| `manifest.json` | Manifesto do PWA (nome, ícones, cores, modo standalone). |
| `sw.js` | Service worker — cache offline e atualização. |
| `icon-192.png` | Ícone do app (192×192). |
| `icon-512.png` | Ícone do app (512×512, também maskable). |

> Os cinco arquivos devem ficar **na mesma pasta**.

---

## Publicação (GitHub Pages)

1. Coloque os 5 arquivos na raiz da pasta servida pelo Pages.
2. O HTML **deve** se chamar `index.html` (o `start_url` e o cache assumem a raiz do diretório).
3. Acesse via **https** (`https://<usuario>.github.io/<repo>/`). O PWA/instalação não funciona em `file://`.

Substituição da versão anterior: faça upload dos arquivos novos sobrescrevendo os antigos, remova arquivos com nomes antigos (ex.: `Sistema_Laudos_ECG.html`) e dê commit. Após ~1 min, abra a URL e faça hard refresh (`Ctrl+Shift+R`); confirme o selo de versão na topbar.

### Cache do service worker (importante a cada update)
A cada nova versão, **incremente o número do cache** em `sw.js` (linha 2):

```js
const CACHE = "ecg-encor-v2";  // -> "ecg-encor-v3" na próxima publicação
```

Sem isso, o service worker pode continuar servindo a versão antiga em cache.

---

## Uso local
Abrir `index.html` no navegador funciona normalmente (edição, banco, PDF). Service worker e instalação ficam desativados em `file://` — isto é esperado.

---

## Notas técnicas
- Front-end puro, sem build. Bibliotecas via CDN: `html2canvas`, `jspdf`, `pdf.js`.
- Persistência: `localStorage` (banco de pacientes e rascunho de sessão). Anexos de imagem ficam apenas em memória.
- Selo de versão = hash MD5 de 8 caracteres, atualizado a cada alteração.
- Validação de desenvolvimento: `node --check` no JS, testes headless (Playwright) das regras do motor, das calculadoras, da comparação e da persistência.

---

## Aviso clínico
Ferramenta de **apoio** à redação de laudos. As regras automáticas (motor de conclusão, calculadoras, alertas de comparação) são auxiliares e não substituem a interpretação e a responsabilidade do médico assinante. Os limiares automáticos seguem convenções usuais e podem ser ajustados conforme a prática do serviço.

---

*Desenvolvido para a Clínica ENCOR — Núcleo de Endocrinologia e Cardiologia · Dr. Genius Costa (CRM-BA 23913).*
