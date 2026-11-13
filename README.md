# wasteflow 
#!/bin/bash
# ============================================================
#  PUBLICAR NO GITHUB — WasteFlow
#  Coloque este arquivo na MESMA pasta do README.md e rode:
#      bash publicar-github.sh
#  (No Windows, use o Git Bash ou o arquivo publicar-github.bat)
# ============================================================

# --- 1. Confere se o Git está instalado ---------------------
if ! command -v git &> /dev/null; then
  echo "❌ Git não encontrado. Instale em https://git-scm.com e rode de novo."
  exit 1
fi

# --- 2. Pede a URL do repositório ---------------------------
# Crie antes o repositório VAZIO no GitHub (botão "New repository"),
# sem marcar "Add a README", e copie a URL dele.
echo "Cole a URL do seu repositório (ex.: https://github.com/seu-usuario/wasteflow.git):"
read -r URL

if [ -z "$URL" ]; then
  echo "❌ Nenhuma URL informada."
  exit 1
fi

# --- 3. Configura nome e e-mail (só se ainda não existirem) --
if [ -z "$(git config --global user.name)" ]; then
  echo "Seu nome (aparece nos commits):"
  read -r NOME
  git config --global user.name "$NOME"
fi

if [ -z "$(git config --global user.email)" ]; then
  echo "Seu e-mail do GitHub:"
  read -r EMAIL
  git config --global user.email "$EMAIL"
fi

# --- 4. Cria o repositório local ----------------------------
# "git init" transforma esta pasta em um repositório Git.
git init

# --- 5. Seleciona os arquivos -------------------------------
# "git add ." inclui tudo da pasta (README.md, código, etc.).
git add .

# --- 6. Registra a versão (commit) --------------------------
git commit -m "Primeira versão do WasteFlow"

# --- 7. Define a branch principal como "main" ---------------
git branch -M main

# --- 8. Liga a pasta ao repositório do GitHub ---------------
# Se já existir um "origin", troca pelo novo endereço.
git remote remove origin 2>/dev/null
git remote add origin "$URL"

# --- 9. Envia para o GitHub ---------------------------------
# O GitHub vai pedir login: use o navegador (que o Git abre)
# ou um Personal Access Token no lugar da senha.
git push -u origin main

echo ""
echo "✅ Pronto! Abra o repositório no navegador para ver o README."
