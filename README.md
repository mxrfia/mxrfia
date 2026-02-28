name: Update README cards

on:
  schedule:
    - cron: "0 3 * * *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Generate GitHub stats SVG
        uses: anuraghazra/github-readme-stats@master
        with:
          username: mxrfia
          show_icons: true
          theme: dracula
          include_all_commits: true
          output_file: profile/stats.svg

      - name: Generate Top Langs SVG
        uses: anuraghazra/github-readme-stats@master
        with:
          username: mxrfia
          layout: compact
          langs_count: 16
          theme: dracula
          output_file: profile/top-langs.svg
          card: top-langs

      - name: Commit & push
        run: |
          mkdir -p profile
          git add profile/*.svg
          git config user.name "github-actions"
          git config user.email "github-actions@users.noreply.github.com"
          git commit -m "Update README cards" || exit 0
          git push
