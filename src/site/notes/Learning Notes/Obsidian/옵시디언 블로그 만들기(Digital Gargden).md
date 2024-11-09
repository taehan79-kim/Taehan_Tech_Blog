---
{"dg-publish":true,"permalink":"/learning-notes/obsidian/digital-gargden/","created":"2024-11-09T13:26:20.867+09:00","updated":"2024-11-09T18:31:03.284+09:00"}
---

# 옵시디언 블로그
Obsidian은 마크다운을 기반으로 한 노트 작성 및 지식 관리 애플리케이션입니다. Digital Garden Plugin과 Obsidian, GitHub, Netlify를 활용하여 옵시디언 내 원하는 노트만을 웹사이트 블로그에 공개하는 법을 작성하였습니다. 
# 준비물
1. Obsidian App 설치([Obsidian](https://obsidian.md/))
2. GitHub 계정([GitHub](https://github.com/))
3. Netlify 계정 연결([Netlify](https://www.netlify.com/))
# 방법
정적 웹사이트를 구축하는 데는 크게 3가지의 요소가 있습니다.
Markdown Files, Static Site Generator, Hosting
저는 Obsidian, Netlify, Github를 사용하였습니다.
1. Obsidian의 Digital Garden Plugin을 설치 후 활성화합니다.
2. [Deploy to Netlify](https://app.netlify.com/start/deploy?repository=https://github.com/oleeskild/digitalgarden)를 눌러 GitHub에 Digital Garden repo를 자신의 repo로 복제합니다. 
   (단, 위 링크는 Netlify용입니다. Vercel을 사용하신다면 [digitalgarden](https://github.com/oleeskild/digitalgarden) 링크로 들어가 Deploy 버튼을 눌러주세요.)
3. 이 후 자신의 깃허브 repo에 자동으로 저장소가 생성된 것을 볼 수 있습니다.
4. Obsidian의 Digital Garden Settings에서 GitHub repo name과 GitHub Username을 자신의 것으로 넣어줍니다.
5. [Create GitHub token](https://github.com/settings/tokens/new?scopes=repo)을 통해 GitHub Token을 생성합니다. Repository access부분에서 Only select repositories를 선택하면 자신의 모든 repo가 아닌 내가 원하는 repo에만 Token을 통해 접근이 가능하여 보안을 지킬 수 있습니다.
6. Permission에서는 Contents와 Pull Requests의 Access 권한을 Read and write로 수정합니다.
7. 만들어진 Token의 password를 복사하여 Obsidian Digital Garden Settings의 GitHub token에 붙여 넣어 줍니다. 
8. 이제 Obsidian 노트의 property에 `dg-publish`와 `dg-home`을 추가하여 노트를 블로그 업로드 할 수 있습니다. 단, `dg-home`은 한 개의 note에만 활성화 되어 있어야 합니다.
9. 이제 cmd + p를 눌러 Digital Garden: Publish 를 눌러 `dg-publish`가 활성화된 노트를 블로그에 업로드를 할 수 있습니다.
