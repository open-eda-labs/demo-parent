# demo-parent

POM pai do app de demonstração: `labs.demo:demo-parent`. Concentra as versões de todas as dependências de terceiros (`dependencyManagement`) e dos plugins (`pluginManagement`), mais a publicação no GitHub Packages. Só dados sintéticos ou públicos.

## Estado

Versão 1.0.0. Etapas do plano: P5 (N1). Decisões relacionadas: D-29, D-39, ADR-0005.

## Contexto

Este repositório faz parte do laboratório open-eda-labs: workflows agênticos disparados por eventos, com Argo Workflows e padrões abertos. Cada peça trocável é um repositório que só depende do contrato versionado em `triage-contract` (D-01, D-27).

A fonte da verdade é o [Plano Mestre](https://github.com/open-eda-labs/open-eda-labs.github.io/blob/main/docs/PLANO_MESTRE.md).

As regras do laboratório valem para todos os repositórios e estão no [perfil da organização](https://github.com/open-eda-labs).

## Regra de ouro (ADR-0005)

A correção de vulnerabilidade nasce aqui ou na [demo-lib](https://github.com/open-eda-labs/demo-lib), nunca no [demo-service](https://github.com/open-eda-labs/demo-service). Uma versão nova é publicada no GitHub Packages e o serviço só sobe a versão que consome. Por isso nenhuma versão de terceiro é declarada fora deste POM.

As versões estão vulneráveis de propósito (lista em `demo-service/docs/dependency-proposal.md`): ignore Dependabot e alertas do GitHub e não corrija fora do pipeline do vuln fixer. As duas transitivas vulneráveis (json-smart via json-path, okio via okhttp) não são fixadas: continuam vindo pela árvore.

## Construir e publicar

JDK 21 e Maven 3.9:

```sh
mvn -B -ntp install    # instala no ~/.m2 local, para construir a demo-lib e o demo-service
```

O push de uma tag `vX.Y.Z` roda `.github/workflows/release.yml` e publica em `https://maven.pkg.github.com/open-eda-labs/demo-parent` com o `GITHUB_TOKEN` do workflow. Ler o pacote de outro repositório exige liberar o acesso de leitura ao pacote nos repositórios da org.
