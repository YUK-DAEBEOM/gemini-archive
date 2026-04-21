# Claude Code CLI 학습·활용 자료 모음

> 2026-04-21 기준, 공신력 있는 블로그·공식 문서·커뮤니티 자료 60+개를 주제별로 정리.

## 1. 공식 문서 (Anthropic)

1. [Claude Code overview (공식)](https://code.claude.com/docs/en/overview) — 전체 소개 및 지원 플랫폼
2. [Best Practices for Claude Code (공식)](https://code.claude.com/docs/en/best-practices) — 모범 사례 정리
3. [Advanced setup (설치)](https://code.claude.com/docs/en/setup) — 플랫폼별 설치·인증
4. [Common workflows](https://code.claude.com/docs/en/common-workflows) — 리팩터·디버그·PR 워크플로우
5. [Claude Code settings.json 레퍼런스](https://code.claude.com/docs/en/settings) — 권한·환경변수 정식 스펙
6. [Hooks reference](https://code.claude.com/docs/en/hooks) — 이벤트/매처/핸들러 구조
7. [MCP 연결 가이드](https://code.claude.com/docs/en/mcp) — MCP 서버 추가 방법
8. [Subagents 공식 문서](https://code.claude.com/docs/en/sub-agents) — 서브에이전트 생성
9. [Skills 공식 가이드](https://code.claude.com/docs/en/skills) — 스킬 작성/로딩
10. [메모리 시스템](https://code.claude.com/docs/en/memory) — CLAUDE.md/자동 메모리 원리
11. [Context window 관리](https://code.claude.com/docs/en/context-window) — 토큰 사용 모니터링
12. [Checkpointing & /rewind](https://code.claude.com/docs/en/checkpointing) — 파일 스냅샷/복구
13. [Headless mode (claude -p)](https://code.claude.com/docs/en/headless) — 비대화식 실행
14. [GitHub Actions 통합](https://code.claude.com/docs/en/github-actions) — PR·이슈 자동화
15. [VS Code 확장](https://code.claude.com/docs/en/vs-code) — 공식 IDE 통합
16. [JetBrains IDE 지원](https://code.claude.com/docs/en/jetbrains) — IntelliJ/PyCharm 등
17. [Output styles](https://code.claude.com/docs/en/output-styles) — 톤/교육 모드
18. [Plugin 디스커버리](https://code.claude.com/docs/en/discover-plugins) — 마켓플레이스 사용
19. [비용 관리 가이드](https://code.claude.com/docs/en/costs) — 프롬프트 캐시·자동 컴팩션
20. [Changelog (공식)](https://code.claude.com/docs/en/changelog) — 릴리스 노트
21. [한국어 Quickstart (공식)](https://code.claude.com/docs/ko/quickstart) — 공식 한국어판
22. [한국어 모범 사례 (공식)](https://code.claude.com/docs/ko/best-practices) — Best practices 한국어

## 2. Anthropic 공식 블로그·리소스

23. [Using CLAUDE.MD files](https://claude.com/blog/using-claude-md-files) — Anthropic 공식 CLAUDE.md 글
24. [Subagents in Claude Code](https://claude.com/blog/subagents-in-claude-code) — 언제·어떻게 서브에이전트 쓰나
25. [Automate security reviews with Claude Code](https://claude.com/blog/automate-security-reviews-with-claude-code) — /security-review 활용
26. [Introduction to agentic coding](https://claude.com/blog/introduction-to-agentic-coding) — 에이전틱 코딩 개념
27. [Enabling Claude Code to work more autonomously](https://www.anthropic.com/news/enabling-claude-code-to-work-more-autonomously) — 자율성 확대 업데이트
28. [Building agents with the Claude Agent SDK](https://claude.com/blog/building-agents-with-the-claude-agent-sdk) — SDK 소개
29. [Claude Code 101 (Skilljar)](https://anthropic.skilljar.com/claude-code-101) — Anthropic 공식 무료 강의
30. [Claude Code power user tips (Help Center)](https://support.claude.com/en/articles/14554000-claude-code-power-user-tips) — 공식 파워유저 팁

## 3. 핵심 베스트 프랙티스 & 종합 가이드

31. [50 Claude Code Tips and Best Practices — Builder.io](https://www.builder.io/blog/claude-code-tips-best-practices) — 50가지 실무 팁
32. [Shipyard cheatsheet (CLI 설정·커맨드·프롬프트)](https://shipyard.build/blog/claude-code-cheat-sheet/) — 치트시트
33. [computingforgeeks cheat sheet](https://computingforgeeks.com/claude-code-cheat-sheet/) — 단축키·명령어
34. [How I use Claude Code — Builder.io](https://www.builder.io/blog/claude-code) — 저자의 워크플로우
35. [Tembo: Mastering Claude Code](https://www.tembo.io/blog/mastering-claude-code-tips) — 생산성 팁 심화
36. [DEV: 20+ hidden tricks ultimate guide](https://dev.to/holasoymalva/the-ultimate-claude-code-guide-every-hidden-trick-hack-and-power-feature-you-need-to-know-2l45) — 숨은 기능
37. [ykdojo/claude-code-tips (GitHub)](https://github.com/ykdojo/claude-code-tips) — 45개 팁 모음
38. [FlorianBruniaux/claude-code-ultimate-guide](https://github.com/FlorianBruniaux/claude-code-ultimate-guide) — 초보→파워유저 템플릿
39. [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) — 바이브→에이전틱 전환

## 4. CLAUDE.md 작성법

40. [HumanLayer: Writing a good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md) — 좋은 CLAUDE.md 설계 원칙
41. [Builder.io: How to write a good CLAUDE.md](https://www.builder.io/blog/claude-md-guide) — 구조·예시
42. [Dometrain: Creating the perfect CLAUDE.md](https://dometrain.com/blog/creating-the-perfect-claudemd-for-claude-code/) — 템플릿
43. [UX Planet — CLAUDE.md 10 섹션](https://uxplanet.org/claude-md-best-practices-1ef4f861ce7c) — 포함할 10개 섹션

## 5. 슬래시 명령·Hooks·Skills·플러그인

44. [BioErrorLog: Custom slash commands](https://en.bioerrorlog.work/entry/claude-code-custom-slash-command) — 커스텀 명령어 만들기
45. [wshobson/commands (GitHub)](https://github.com/wshobson/commands) — 실전 슬래시 명령 컬렉션
46. [DataCamp: Claude Code Hooks 가이드](https://www.datacamp.com/tutorial/claude-code-hooks) — 훅 자동화
47. [disler/claude-code-hooks-mastery](https://github.com/disler/claude-code-hooks-mastery) — 훅 마스터 레포
48. [DEV: 20+ ready-to-use hook examples](https://dev.to/lukaszfryc/claude-code-hooks-complete-guide-with-20-ready-to-use-examples-2026-dcg) — 실전 예시
49. [FreeCodeCamp: Build your own Claude Code Skill](https://www.freecodecamp.org/news/how-to-build-your-own-claude-code-skill/) — 스킬 제작 튜토리얼
50. [Towards Data Science: Production-ready Skill](https://towardsdatascience.com/how-to-build-a-production-ready-claude-code-skill/) — 고급 스킬 설계
51. [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) — 최대 규모 awesome 리스트
52. [hesreallyhim/awesome-claude-code-output-styles](https://github.com/hesreallyhim/awesome-claude-code-output-styles-that-i-really-like) — 출력 스타일 모음

## 6. MCP 서버·IDE 통합

53. [Scott Spence: Configuring MCP tools the better way](https://scottspence.com/posts/configuring-mcp-tools-in-claude-code) — 설정 파일 직접 편집 팁
54. [Builder.io: MCP 서버 설정](https://www.builder.io/blog/claude-code-mcp-servers) — 연결·구성 방법
55. [Docker: MCP Toolkit 연동](https://www.docker.com/blog/add-mcp-servers-to-claude-code-with-mcp-toolkit/) — 컨테이너화 MCP
56. [JetBrains AI 블로그: Claude Agent in JetBrains](https://blog.jetbrains.com/ai/2025/09/introducing-claude-agent-in-jetbrains-ides/) — IntelliJ 네이티브 통합

## 7. 서브에이전트·병렬화·Worktree

57. [Pragmatic Engineer: How Claude Code is built](https://newsletter.pragmaticengineer.com/p/how-claude-code-is-built) — 내부 구조 (Gergely Orosz)
58. [Lenny's Newsletter: Boris Cherny 인터뷰](https://www.lennysnewsletter.com/p/head-of-claude-code-what-happens) — 제작자 인터뷰
59. [howborisusesclaudecode.com](https://howborisusesclaudecode.com/) — Boris 본인의 워크플로우
60. [Tim Dietrich: Sub-agents for parallel work](https://timdietrich.me/blog/claude-code-parallel-subagents/) — 병렬 패턴
61. [MindStudio: 5 agentic workflow patterns](https://www.mindstudio.ai/blog/claude-code-agentic-workflow-patterns) — 워크플로우 5패턴
62. [Dan Does Code: Parallel vibe coding with worktrees](https://www.dandoescode.com/blog/parallel-vibe-coding-with-git-worktrees) — worktree 실전
63. [Medium: Git worktrees for parallel dev](https://medium.com/@dtunai/mastering-git-worktrees-with-claude-code-for-parallel-development-workflow-41dc91e645fe) — 세부 셋업

## 8. Plan 모드·컨텍스트·메모리

64. [Armin Ronacher: What Actually Is Plan Mode?](https://lucumr.pocoo.org/2025/12/17/what-is-plan-mode/) — 깊이 있는 분석
65. [DataCamp: Plan Mode design-review 루프](https://www.datacamp.com/tutorial/claude-code-plan-mode) — 리팩터링 루프
66. [codewithmukesh: Plan mode 활용](https://codewithmukesh.com/blog/plan-mode-claude-code/) — "Think Before You Build"
67. [charliegleason: Understanding context windows](https://code.charliegleason.com/understanding-context-windows) — 200k/1M 토큰 전략
68. [MindStudio: 18 token management hacks](https://www.mindstudio.ai/blog/claude-code-token-management-hacks-3) — 세션 연장 팁
69. [MadPlay: Context & token optimization](https://madplay.github.io/en/post/claude-code-context-and-token-optimization) — 컨텍스트 축소 전략
70. [Claude Directory: Auto-memory 가이드](https://www.claudedirectory.org/blog/claude-code-auto-memory-guide) — 자동 메모리 심화
71. [Product Talk: Give Claude Code a memory](https://www.producttalk.org/give-claude-code-a-memory/) — 반복 지시 줄이기

## 9. TDD·디버깅·리팩터·보안·리뷰

72. [The New Stack: TDD with Claude Code](https://thenewstack.io/claude-code-and-the-art-of-test-driven-development/) — TDD 통합
73. [alexop.dev: Agentic Red-Green-Refactor loop](https://alexop.dev/posts/custom-tdd-workflow-claude-code-vue/) — TDD 강제 워크플로우
74. [Steve Kinney: TDD with Claude](https://stevekinney.com/courses/ai-development/test-driven-development-with-claude) — 강의형 가이드
75. [Milvus: Can Claude refactor legacy code?](https://milvus.io/ai-quick-reference/can-claude-code-refactor-legacy-code) — 레거시 코드 리팩터
76. [DataCamp: Claude Code practical examples](https://www.datacamp.com/tutorial/claude-code) — 실습 튜토리얼

## 10. CI/CD·Headless·자동화

77. [systemprompt.io: Claude Code GitHub Actions](https://systemprompt.io/guides/claude-code-github-actions) — PR 리뷰·CI 셋업
78. [MindStudio: Headless mode 가이드](https://www.mindstudio.ai/blog/claude-code-headless-mode-autonomous-agents-2) — 무대화식 실행
79. [DataCamp: Claude Agent SDK 튜토리얼](https://www.datacamp.com/tutorial/how-to-use-claude-agent-sdk) — SDK로 커스텀 에이전트

## 11. 비교·엔터프라이즈

80. [Cosmic JS: Claude vs Copilot vs Cursor 2026](https://www.cosmicjs.com/blog/claude-code-vs-github-copilot-vs-cursor-which-ai-coding-agent-should-you-use-2026) — 최신 비교
81. [Uvik: 2026 Usage Report](https://uvik.net/blog/claude-code-vs-cursor-vs-copilot-vs-codex-2026/) — 실사용 리포트
82. [CodeAgents: Enterprise 배포 전략](https://codeagents.app/guides/enterprise-deployment) — 대규모 도입

## 12. 한국어 자료 (velog·블로그·기술 블로그)

83. [컬리 기술 블로그: 예측 가능한 바이브 코딩](https://helloworld.kurly.com/blog/vibe-coding-with-claude-code/) — 국내 대기업 실무 사례
84. [하이퍼리즘 기술 블로그: Claude Code 사용 가이드](https://tech.hyperithm.com/claude_code_guides) — 종합 가이드
85. [DevOcean(SK): 진짜 AI 에이전트형 코딩 툴](https://devocean.sk.com/blog/techBoardDetail.do?ID=167718&boardType=techBlog) — 국내 기술 블로그 리뷰
86. [F-Lab: Claude Code 실전 가이드 (Gotama 멘토)](https://f-lab.ai/en/blog/article-claude-code-guide-20250731) — AI 네이티브 개발
87. [velog @skysoo: Claude Code 완벽 가이드 한글 요약본](https://velog.io/@skysoo/Claude-Code-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C-%ED%95%9C%EA%B8%80-%EC%9A%94%EC%95%BD%EB%B3%B8) — 공식 문서 요약
88. [velog @okorion: 45가지 실전 전략](https://velog.io/@okorion/Claude-Code%EB%A5%BC-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%93%B0%EB%8A%94-%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98-45%EA%B0%80%EC%A7%80-%EC%8B%A4%EC%A0%84-%EC%A0%84%EB%9E%B545-Claude-Code-Tips-From-Basics-to-Advanced-%EC%9A%94%EC%95%BD) — ykdojo 팁 한글 정리
89. [velog @tri2601: 공식 Best Practices 번역](https://velog.io/@tri2601/%ED%81%B4%EB%A1%9C%EB%93%9C-%EC%BD%94%EB%93%9C-%EA%B3%B5%EC%8B%9D-%EB%AC%B8%EC%84%9C-4.-Best-Practices-for-Claude-Code) — 공식 문서 번역
90. [PyTorchKR: 30+ Claude Code 활용 팁](https://discuss.pytorch.kr/t/30-claude-code-feat-ykdojo-claude-code-tips/8368) — 커뮤니티 정리
91. [CC101: Claude Code 한국어 입문 가이드](https://cc101.axwith.com/ko) — 체계적 입문서
92. [revfactory/claude-code-mastering (GitHub)](https://github.com/revfactory/claude-code-mastering) — 국내 개발자 마스터 레포
93. [이랜서 블로그: Claude Code 사용법](https://www.elancer.co.kr/blog/detail/1012) — 도입 후기
94. [blogtechnicus: 2026 Claude Code 사용법 완벽 가이드](https://blogtechnicus.com/claude-code-%EC%82%AC%EC%9A%A9%EB%B2%95/) — 종합 한글 가이드

## 13. 추가 참고·커뮤니티

95. [ClaudeLog: 전체 설정/메커니즘 허브](https://claudelog.com/configuration/) — 세부 설정 심층 분석
96. [Till Freitag: 15 hidden features](https://till-freitag.com/en/blog/claude-code-hidden-features-en) — 숨은 기능 15선
97. [DEV: Complete power user guide (slash/hooks/skills)](https://dev.to/numbpill3d/the-complete-claude-code-power-user-guide-slash-commands-hooks-skills-more-6ep) — 종합 파워유저
98. [VGV Wingspan: 오픈소스 에이전틱 워크플로우](https://verygood.ventures/blog/vgv-wingspan-agentic-engineering-workflow/) — 엔지니어링 워크플로우
99. [Piebald-AI/claude-code-system-prompts](https://github.com/Piebald-AI/claude-code-system-prompts) — 실제 시스템 프롬프트 해부
100. [rohitg00/awesome-claude-code-toolkit](https://github.com/rohitg00/awesome-claude-code-toolkit) — 135 에이전트·42 명령 등 초대형 툴킷

---

## 주제별 학습 권장 순서

1. **입문**: 공식 Overview(1) → Quickstart/Setup(3) → 한국어 Quickstart(21) → CC101(91)
2. **CLAUDE.md**: HumanLayer(40) → Builder.io(41) → 하이퍼리즘(84)
3. **컨텍스트 관리**: charliegleason(67) → MindStudio token hacks(68) → MadPlay(69)
4. **Plan 모드 & 워크플로우**: Armin Ronacher(64) → DataCamp(65) → Boris 인터뷰(58)
5. **확장(Hooks/Skills/MCP)**: DataCamp hooks(46) → FreeCodeCamp skills(49) → Scott Spence MCP(53)
6. **자동화(Headless/CI)**: 공식 headless(13) → GitHub Actions(77) → MindStudio headless(78)
7. **파워유저**: ykdojo(37) → Boris Cherny 인터뷰(58) → Till Freitag(96)

## 사용 팁

- 공식 문서(1~22)는 가장 최신·정확하므로 먼저 참조
- 한국어 자료(83~94)는 국내 개발 환경 이슈(Git Bash, WSL, 한글 응답 설정 등)에 특히 유용
- Awesome 리스트(51, 100)는 실전 템플릿/스킬/훅 바로 가져다 쓸 때 활용
- Boris Cherny 인터뷰 계열(57~59)은 "5개 세션 병렬" 등 최전선 활용법을 다룸
