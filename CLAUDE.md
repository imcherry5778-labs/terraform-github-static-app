# CLAUDE.md

## 프로젝트 개요

HCP Waypoint Template으로 사용하기 위한 Terraform 모듈.
GitHub 저장소를 템플릿에서 자동 생성하고, GitHub Pages를 활성화하며, Slack webhook secret을 설정한다.

원본: `hashicorp-education/terraform-github-static-app`

## 구조

```
main.tf           # 핵심 리소스 (github_repository, github_repository_file, github_actions_environment_secret)
variables.tf      # 입력 변수 (gh_token, destination_org, waypoint_application 등)
outputs.tf        # 출력값 (repo_url, app_url, repo_name)
templates/        # 생성될 저장소의 README.md 템플릿
```

## 주요 변수

| 변수 | 설명 | 필수 |
|------|------|------|
| `gh_token` | GitHub PAT (scope: `repo`, `delete_repo`, `workflow`) | Yes |
| `destination_org` | 저장소가 생성될 GitHub 조직 | Yes (default: `hashicorp-education`) |
| `waypoint_application` | 생성할 앱/저장소 이름 (하이픈, 언더스코어 불가) | Yes |
| `template_org` | 템플릿 저장소가 있는 조직 | No (default: `imcherry5778-labs`) |
| `template_repo` | 템플릿 저장소 이름 | No (default: `learn-hcp-waypoint-static-app-template`) |
| `slack_hook_url` | Slack webhook URL | No |

## Waypoint 연동 흐름

1. 이 모듈을 HCP Terraform Registry에 No-Code 모듈로 등록
2. HCP Waypoint에서 Template 생성 시 이 모듈을 연결
3. 개발자가 Waypoint에서 앱을 생성하면 Terraform이 GitHub 저장소 + Pages를 자동 프로비저닝

**참고**: No-Code 모듈은 HCP Terraform Standard 이상에서만 지원. Free tier에서는 표준 모듈로 등록 가능.

## Commit Convention

```
<prefix>: <한글 설명>
```

Prefix: `feat`, `fix`, `docs`, `refactor`, `chore`, `test`

## 명령어

```bash
terraform init      # 프로바이더 설치
terraform validate  # 구문 검증
terraform fmt       # 포맷팅
terraform plan      # 변경사항 미리보기
terraform apply     # 적용
```
