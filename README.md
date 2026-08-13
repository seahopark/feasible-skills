# feasible-skills

[Feasible Research](https://research.feasible.kr)를 만들며 실제로 쓰고 다듬어온 PM/QA 워크플로우를 [Claude Code / OpenClaw Skills](https://code.claude.com/docs/en/skills) 포맷으로 정리한 저장소입니다. 특정 프로젝트에 종속된 부분은 빼고, 다른 제품/팀에도 그대로 옮겨 쓸 수 있는 것만 골라 담았습니다.

## 스킬 목록

| 스킬 | 용도 | 사용 시점 |
|---|---|---|
| [`user-story-mapping`](skills/user-story-mapping/SKILL.md) | 유저 여정 기반 스토리 맵 작성 (Jeff Patton 프레임워크) | 백로그/MVP 범위를 유저 워크플로우 축으로 정리할 때 |
| [`product-spec`](skills/product-spec/SKILL.md) | 스크린샷·실제 사용 기반 역기획 → Business Discovery + Product Spec 두 문서로 구조화 | 기존 제품의 기능 정의가 문서화돼 있지 않을 때 |
| [`feasible-lab-prd`](skills/feasible-lab-prd/SKILL.md) | AC(Acceptance Criteria) 중심 PRD 초안 생성 | 새 기능/프로젝트 기획을 마크다운 PRD로 빠르게 초안 잡을 때 |
| [`functional-tc-authoring`](skills/functional-tc-authoring/SKILL.md) | 실제 프론트엔드 코드를 근거로 수동 QA용 기능 테스트케이스 시트 작성 | 코드 기준으로 정확한 TC를 새로 뽑아야 할 때 |
| [`policy-doc`](skills/policy-doc/SKILL.md) | 서비스 정책 문서(PRD/TC → policy.md) 생성 및 코드 대조 검증 | 기획과 실제 구현이 어긋나 있는지 확인하며 정책서를 관리할 때 |

각 스킬은 `SKILL.md`에 상세 절차가 있고, 일부는 `references/`에 템플릿을 같이 둡니다.

## 사용법

Claude Code나 OpenClaw를 쓴다면 원하는 스킬 폴더를 그대로 워크스페이스의 스킬 디렉토리에 복사하면 됩니다.

```bash
git clone https://github.com/<your-org>/feasible-skills.git
cp -r feasible-skills/skills/product-spec ~/your-workspace/skills/
```

## License

MIT — 자유롭게 가져다 쓰고 변형하세요.
