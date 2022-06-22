# GitHub v Azure DevOps
GitHub
Enterprise
L Organization A
 L Repository 1
  L Pipeline あ
  L Pipeline い
 L Repository 2
  L Pipeline う
 L Repository 3
     L Pipeline え
L Organization B
 L Repository 4
  L Pipeline お

Azure DevOps
L Organization A
 L Project X
  L Repository 1
   L Pipeline あ
      L Pipeline い
  L Repository 2
   L Pipeline う
 L Project Y
  L Repository 3
      L Pipeline え
L Organization B
 L Project Z
  L Repository 4
   L Pipeline お

# OpenHack
Q&A:
Q) OpenHackのチャレンジ内容はどこにある？
A) 
- https://aka.ms/devopsopenhackv3challenges
- 我々が昨日作成したOrgにデプロイしたもの、https://github.com/DevOps-OpenHack-DryRun/ChallengeDocs

Q) OpenHackに必要なGitHub環境はどうやって準備できる?
A) 
- Enterprise契約相当のものを用意
- EnterpriseのFree TrialでChallenge 3までできそう（厳密にはChallenge 3は昨日は終わらなかったが）
 - https://github.com/pricing
 - https://docs.github.com/en/get-started/signing-up-for-github/setting-up-a-trial-of-github-enterprise-cloud
 - 我々が昨日（2022/6/14)に作成したOrganizationは: https://github.com/DevOps-OpenHack-DryRun

Q) GitHubのFreeだと、OpenHackでできないことは?
A) 例えば、
- ブランチポリシーの設定

Q) OpenHackのコンテンツをGitHubとAzureにデプロイする方法は？
A)
https://github.com/microsoft/OpenHack/tree/main/byos
https://github.com/microsoft/OpenHack/blob/main/byos/devops/GH.md

Q) 何がデプロイされる？
A)
- GitHubのOrganization内に"apis/"や"iac"等のC#,Java,JavaScript,Go,Bicep,Terraform等のコードやその他ファイルがデプロイされる
- Azure上には、リソースグループとStorageがデプロイされる（Storageの中にはOpenHackに必要なcredentialsが保存されているという理解）

Q) 我々が昨日から使っているDryRunレポは?
A) https://github.com/DevOps-OpenHack-DryRun/devopsoh83241

Q) ghはどうやってインストールする？
A）Linuxの場合は: https://github.com/cli/cli/blob/trunk/docs/install_linux.md

Q) iac/bicep内のdeploy.shを実行すると"declare: not found"というエラーが出るのはなぜ？
A) 昨日は答えが分からなかった。ただし、少なくともChallege 3までは、deploy.shを実行する必要はない

Q) 管理者がmainブランチにpushできちゃうのを防ぐには？
A) "Include administrators"をチェック