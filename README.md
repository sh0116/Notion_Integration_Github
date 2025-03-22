# GitHub ↔ Notion Sync

## 📌 개요
이 프로젝트는 **GitHub Issues**와 **Notion**을 자동으로 동기화하는 Python 스크립트입니다. 

### ✨ 주요 기능
1. **GitHub → Notion 동기화**
   - GitHub에서 Issue가 생성되거나 업데이트되면, Notion의 데이터베이스에 해당 내용이 자동으로 추가됨
   - Issue의 제목, 본문, 링크, 상태(State) 등을 Notion에 저장

2. **Notion → GitHub 동기화**
   - Notion에서 새롭게 작성된 페이지를 5분마다 GitHub에 Markdown 파일로 저장
   - Notion 페이지의 제목, 내용, 테이블 등을 Markdown 형식으로 변환하여 GitHub에 Commit

---

## 🚀 사용 방법

### 1️⃣ 환경 변수 설정
다음 환경 변수를 설정해야 합니다:
- GITHUB Actions 환경 변수 설정
- settings > Secrets and variables > actions
- (https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables)

```bash
PERSONAL_GITHUB_ACCESS_KEY=your_github_access_token
REPO_OWNER=your_github_username
REPO_NAME=your_repository_name
NOTION_KEY=your_notion_integration_token
NOTION_DATABASE_ID=your_notion_database_id
```

---

## 🔄 GitHub Actions 자동화
### GitHub Issue 생성 시 Notion으로 동기화
GitHub Issue가 생성되거나 상태가 변경될 때 자동으로 Notion에 반영됩니다.

`.github/workflows/sync_github_to_notion.yml`:

```yaml
name: Sync GitHub Issues to Notion

on:
  issues:
    types: [opened, reopened, closed, deleted]

jobs:
  sync_github_to_notion:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install dependencies
        run: |
          pip install --upgrade pip
          pip install -r github-issues-integration-notion_src/requirements.txt

      - name: Run sync_github_to_notion
        run: python github-issues-integration-notion_src/main.py sync_github_to_notion
        env:
          PERSONAL_GITHUB_ACCESS_KEY: ${{ secrets.PERSONAL_GITHUB_ACCESS_KEY }}
          REPO_OWNER: ${{ github.repository_owner }}
          REPO_NAME: ${{ github.event.repository.name }}
          NOTION_KEY: ${{ secrets.NOTION_KEY }}
          NOTION_DATABASE_ID: ${{ secrets.NOTION_DATABASE_ID }}
```

### Notion 페이지를 5분마다 GitHub에 동기화
5분마다 Notion 페이지를 GitHub에 Markdown 파일로 저장합니다.

`.github/workflows/sync_notion_to_github.yml`:

```yaml
name: Sync Notion to GitHub

on:
  schedule:
    - cron: '*/5 * * * *'

jobs:
  sync_notion_to_github:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install dependencies
        run: |
          pip install --upgrade pip
          pip install -r github-issues-integration-notion_src/requirements.txt

      - name: Run sync_notion_to_github
        run: python github-issues-integration-notion_src/main.py sync_notion_to_github
        env:
          PERSONAL_GITHUB_ACCESS_KEY: ${{ secrets.PERSONAL_GITHUB_ACCESS_KEY }}
          REPO_OWNER: ${{ github.repository_owner }}
          REPO_NAME: ${{ github.event.repository.name }}
          NOTION_KEY: ${{ secrets.NOTION_KEY }}
          NOTION_DATABASE_ID: ${{ secrets.NOTION_DATABASE_ID2 }}
```

---

## 📌 코드 설명
### 🔹 `sync_github_to_notion()`
- GitHub에서 Issue를 가져와 Notion 데이터베이스에 추가/업데이트
- Issue의 제목, 본문, 생성일, 상태(State) 등을 저장

### 🔹 `sync_notion_to_github()`
- Notion에서 페이지를 가져와 Markdown 형식으로 변환 후 GitHub에 커밋
- 페이지가 5분 이내에 수정된 경우에만 업데이트

### 🔹 `fetch_page_blocks(page_id)`
- Notion의 페이지 내용을 블록 단위로 가져와 Markdown으로 변환

### 🔹 `convert_rich_text_to_markdown(rich_text)`
- Notion의 리치 텍스트를 Markdown 스타일로 변환
- 굵게, 기울임, 밑줄, 코드 블록 등을 지원

### 🔹 `fetch_table_blocks(table_block_id)`
- Notion 테이블을 Markdown 표로 변환

---

## 🛠️ 기술 스택
- **언어**: Python 3.10
- **라이브러리**:
  - `requests` (Notion API 호출)
  - `notion_client` (Notion API 연결)
  - `PyGithub` (GitHub API 연동)
- **GitHub Actions**: 자동화된 동기화 실행

---


<!-- Contributing -->
## Contributing

<a href="https://github.com/sh0116">
  <img width=100 height=100 src="https://avatars.githubusercontent.com/u/38518675?v=4" />
</a>


<!-- Code of Conduct -->
### Code of Conduct

Please read the [Code of Conduct](https://github.com/Louis3797/awesome-readme-template/blob/master/CODE_OF_CONDUCT.md)


## 📜 라이선스
이 프로젝트는 MIT 라이선스를 따릅니다.


<!-- Contact -->
## Contact

Email : seokhyeon116@naver.com
