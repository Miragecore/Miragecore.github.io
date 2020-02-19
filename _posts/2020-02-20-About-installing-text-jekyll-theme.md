---
title: TeXt Jekyll Theme 설치시 작업
tags: TeXt Jekyll
---
TeXt Theme를 설치하면서 한 일들
1. TeXt Theme repoitory에서 Folk
2. Folk된 github내 repository의 setting에서 Repository name을 Miragecore.github.io로 변경
2. PC로 git Clone 
3. 로컬 repo 안의 Gemfile에 타임존 데이터 관련 gem 추가 
   
   타임존을 Asia/Seoul 로 변경시 타임존 데이터를 읽어오지 못하는 현상을 해결하기 위함.
```
gem 'tzinfo'
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]
```
4. gem 설치 (Ruby 설치는 이미 해두었던 상태)
    
    --path 옵션이 deprecated 되었다는 메세지를 떳지만 설치는 되었음.    
```console
bundle install --path vendor/bundle
```
5. _config.yml 에서 필요한 항목 수정 및 아래 theme 관련 항목 추가
```
theme: jekyll-text-theme
```

6. 로컬에서 jekyll 실행
```
bundle exec jekyll serve
```

7. 로컬 사이트 접속
```
http://127.0.0.1:4000
```

Post 태그를 2개 이상 설정시에는 스페이스로 구분한다.