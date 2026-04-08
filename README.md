# terraform-github-static-app

GitHub 저장소를 템플릿에서 자동 생성하고 GitHub Pages를 활성화하는 Terraform 모듈.
HCP Waypoint Template으로 사용하여 개발자 셀프서비스 플랫폼을 구축할 수 있다.

원본: [hashicorp-education/terraform-github-static-app](https://github.com/hashicorp-education/terraform-github-static-app)

## 사전 요구사항

- Terraform >= 1.0
- GitHub Personal Access Token (필요 scope: `repo`, `delete_repo`, `workflow`)
- GitHub 조직 (저장소 생성 대상)
- 템플릿 저장소: [`learn-hcp-waypoint-static-app-template`](https://github.com/imcherry5778-labs/learn-hcp-waypoint-static-app-template)

## 리소스

이 모듈이 생성하는 리소스:

- `github_repository` — 템플릿 기반 저장소 (GitHub Pages 활성화)
- `github_repository_file` — README.md 파일
- `github_actions_environment_secret` — Slack webhook URL secret

## 변수

| 이름 | 설명 | 타입 | 기본값 | 필수 |
|------|------|------|--------|------|
| `gh_token` | GitHub PAT | `string` | - | Yes |
| `destination_org` | 저장소 생성 대상 조직 | `string` | `"hashicorp-education"` | No |
| `waypoint_application` | 앱/저장소 이름 (`-`, `_` 사용 불가) | `string` | - | Yes |
| `template_org` | 템플릿 저장소 조직 | `string` | `"imcherry5778-labs"` | No |
| `template_repo` | 템플릿 저장소 이름 | `string` | `"learn-hcp-waypoint-static-app-template"` | No |
| `slack_hook_url` | Slack webhook URL | `string` | `""` | No |

## 출력

| 이름 | 설명 |
|------|------|
| `repo_url` | 생성된 저장소 URL |
| `app_url` | GitHub Pages 앱 URL |
| `repo_name` | 저장소 전체 이름 (`org/repo`) |

## 사용 방법

### 직접 사용

```hcl
module "static_app" {
  source = "github.com/imcherry5778-labs/terraform-github-static-app"

  gh_token             = var.gh_token
  destination_org      = "imcherry5778-labs"
  waypoint_application = "mystaticapp"
}
```

### HCP Terraform + Waypoint 연동

1. HCP Terraform Registry에 이 모듈을 등록 (No-Code 모듈)
2. HCP Waypoint에서 Template 생성 시 등록된 모듈 연결
3. 개발자가 Waypoint UI에서 앱 생성 -> Terraform이 저장소 + Pages 자동 프로비저닝

## Free Tier 제한 사항

- **No-Code 모듈**은 HCP Terraform **Standard 이상**에서만 지원
- Free tier에서는 **표준 모듈**로 Registry에 등록하여 사용 가능 (No-Code 워크플로우 불가)
- Waypoint Template 연동 시 No-Code 모듈이 필요하므로, Free tier에서는 Waypoint 자동 프로비저닝이 제한됨
