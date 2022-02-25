# Original Docs
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository

# Azure DevOps
https://samlearnsazure.blog/2020/06/22/cleaning-up-secrets-in-git/

# Notes
Historyの書き換え/削除はベストプラクティスに反すると言えると思う
- Sensitive dataが流出している場合は、historyを書き換え/削除しても意味がない
- Historyを書き換えることは、Historyの信ぴょう性を阻害する
- それ関連のdocsが充実していないのは、ベストプラクティスに反するからだと思われる
  - Azure DevOps関連のdocsはない
  - GitHub関連のdocsはあるが、そこで推奨しているツールの提供元はマイクロソフトではない