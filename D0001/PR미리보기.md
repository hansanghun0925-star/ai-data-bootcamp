# (맛보기) 작업용 브랜치를 하나 만들어보기
git switch -c practice-pr
# ... 파일을 조금 고친 뒤 ...
git add . && git commit -m "practice: PR 흐름 연습"
git push origin practice-pr
# 그다음 GitHub 저장소 페이지에 뜨는 'Compare & pull request' 버튼을 눌러 PR을 열어봅니다.