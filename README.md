name: Generate Snake Animation

on:
  schedule:
    # Roda a cada 12 horas para manter a animação atualizada
    - cron: "0 */12 * * *"
  # Permite rodar manualmente através da interface do GitHub Actions
  workflow_dispatch:

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      # 1. Checa o repositório
      - uses: actions/checkout@v4
      
      # 2. Gera os arquivos SVG do snake
      - uses: Platane/snk/svg-only@v3 
        with:
          # Seu nome de usuário do GitHub (IMPRESCINDÍVEL)
          github_user_name: playbet4 
          outputs: |
            dist/github-snake.svg
            dist/github-snake-dark.svg
          
      # 3. Envia os arquivos gerados para o branch 'output'
      - name: Push Snake SVG to Git
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output # Cria ou atualiza o branch 'output'
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
