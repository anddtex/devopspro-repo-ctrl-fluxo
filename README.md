# DEVOPS PRO repo-ctrl-fluxo 
Repositorio do curso DevOps Pro aula de Workflow e dispatch

# GitHub Actions — Workflow Dispatch

> Guia prático baseado na transcrição da aula sobre o evento `workflow_dispatch`.

## 📌 Visão geral

O `workflow_dispatch` permite executar manualmente um workflow do GitHub Actions. É útil durante o desenvolvimento e os testes de uma automação, ou quando a decisão de iniciar a execução deve ser manual.

A aula recomenda habilitar esse evento durante o desenvolvimento para testar o workflow sem precisar disparar repetidamente o evento que será usado no fluxo definitivo. Se a execução manual não for necessária posteriormente, o evento pode ser removido.

## ⚙️ 1. Declarando o evento

Declare `workflow_dispatch` na seção `on` do arquivo YAML:

```yaml
name: Workflow Evento

on:
  workflow_dispatch:
```

Na demonstração, o workflow não é iniciado automaticamente por eventos como um pull request ou envio de código. Ele é executado quando alguém o inicia manualmente.

## 🧪 2. Exemplo básico

```yaml
name: Workflow Evento

on:
  workflow_dispatch:

jobs:
  teste:
    runs-on: ubuntu-latest

    steps:
      - name: Action de teste
        run: echo "Disparou o evento"
```

### Entendendo o exemplo

- `name`: nome do workflow.
- `on.workflow_dispatch`: habilita o disparo manual.
- `jobs`: declara os trabalhos da automação.
- `runs-on: ubuntu-latest`: define o runner utilizado no exemplo.
- `steps`: lista as etapas do job.
- `run`: executa um comando de shell; neste caso, `echo` imprime uma mensagem.

## ▶️ 3. Executando pela interface do GitHub

Depois de salvar o arquivo e fazer commit:

1. Acesse a aba **Actions** do repositório.
2. Selecione o workflow.
3. Clique em **Run workflow**.
4. Inicie a execução manual.

O GitHub inicia o workflow, aloca um runner para executar o job e apresenta o resultado.

Saída esperada do exemplo:

```text
Disparou o evento
```

## 🧩 4. Utilizando Inputs

Além de iniciar um workflow manualmente, `workflow_dispatch` permite declarar Inputs para receber valores durante a execução. Isso ajuda a parametrizar e reutilizar uma mesma automação.

A aula relembra quatro tipos de Inputs:

| Tipo | Finalidade |
|---|---|
| `string` | Texto |
| `number` | Número |
| `boolean` | Verdadeiro ou falso |
| `choice` | Seleção entre opções predefinidas |

### Exemplo com um Input textual

```yaml
name: Workflow com Input

on:
  workflow_dispatch:
    inputs:
      exemplo_input:
        type: string
        description: Teste de input
        required: true

jobs:
  teste:
    runs-on: ubuntu-latest

    steps:
      - name: Action de teste
        run: echo "${{ inputs.exemplo_input }}"
```

Neste exemplo:

- `exemplo_input` é o nome do Input.
- `type: string` define uma entrada textual.
- `description` descreve a entrada para o usuário.
- `required: true` indica que a entrada é obrigatória.
- `${{ inputs.exemplo_input }}` acessa o valor informado.

## 📝 5. Exemplo de execução

Se o usuário preencher o campo com:

```text
exemplo de entrada
```

o step abaixo:

```yaml
- name: Action de teste
  run: echo "${{ inputs.exemplo_input }}"
```

imprime:

```text
exemplo de entrada
```

Assim, a mesma automação pode receber valores diferentes sem que o YAML precise ser alterado a cada execução.

## 🔄 6. Com Inputs ou sem Inputs

### Sem Inputs

Use quando basta iniciar manualmente o workflow, utilizando as configurações já declaradas ou os contextos disponíveis.

```yaml
on:
  workflow_dispatch:
```

### Com Inputs

Use quando a execução precisa receber parâmetros do usuário.

```yaml
on:
  workflow_dispatch:
    inputs:
      exemplo_input:
        type: string
        description: Teste de input
        required: true
```

O uso de Inputs é opcional; depende das necessidades da automação.

## 🛠️ 7. Quando utilizar `workflow_dispatch`?

A aula destaca dois cenários principais.

### Desenvolvimento e testes

Permite validar uma pipeline sem precisar gerar o evento que será utilizado no fluxo definitivo.

### Execução decidida manualmente

É apropriado quando uma pessoa deve decidir quando iniciar a automação, em vez de ela ser iniciada automaticamente por outro evento.

## 🧠 Fluxo de execução

```text
Usuário acessa Actions
          │
          ▼
Seleciona o workflow
          │
          ▼
Clica em Run workflow
          │
          ▼
Preenche Inputs, se existirem
          │
          ▼
GitHub inicia a execução
          │
          ▼
Runner executa Jobs e Steps
          │
          ▼
Resultado da execução
```

## ⚠️ Pontos importantes

- Declare `workflow_dispatch` dentro de `on`.
- Na demonstração, o workflow só é executado quando iniciado manualmente.
- Inputs são opcionais e podem ser adicionados para parametrizar a execução.
- Utilize o contexto `inputs` para acessar os valores fornecidos.
- Durante o desenvolvimento, a execução manual facilita os testes.
- Se a execução manual não for necessária no fluxo final, a aula observa que o evento pode ser removido.

## 📋 Checklist

- [ ] O evento está declarado dentro de `on`.
- [ ] O workflow contém os jobs e steps necessários.
- [ ] O runner foi definido em `runs-on`.
- [ ] O arquivo foi salvo no repositório.
- [ ] A execução foi testada pela aba **Actions**.
- [ ] Se houver Inputs, eles estão declarados dentro de `workflow_dispatch.inputs`.
- [ ] Os valores são acessados por `${{ inputs.nome_do_input }}`.
- [ ] Foi avaliado se o disparo manual continuará necessário após o desenvolvimento.

## 📚 Resumo

O `workflow_dispatch` é utilizado para iniciar workflows manualmente no GitHub Actions.

Declaração mínima:

```yaml
on:
  workflow_dispatch:
```

Exemplo com Input:

```yaml
on:
  workflow_dispatch:
    inputs:
      exemplo_input:
        type: string
        description: Teste de input
        required: true
```

Acesso ao valor:

```yaml
${{ inputs.exemplo_input }}
```

> **Conceito principal:** utilize `workflow_dispatch` quando quiser controlar manualmente o início de uma automação, especialmente durante o desenvolvimento, os testes ou em processos que exigem uma decisão explícita de execução.

---

## 📖 Referência

Documentação estruturada a partir da transcrição fornecida da aula sobre **Workflow Dispatch**, preservando os conceitos e exemplos apresentados.

# GitHub Actions — Evento `push`

## Objetivo

O evento `push` dispara um workflow do GitHub Actions quando alterações são enviadas ao repositório. Ele pode ser configurado para executar em qualquer branch ou apenas em branches, tags e caminhos específicos.

## 1. Configuração básica

Para executar o workflow a cada `push`, independentemente da branch:

```yaml
name: Action Teste

on:
  push:

jobs:
  teste:
    runs-on: ubuntu-latest
    steps:
      - name: Exibir mensagem
        run: echo "Workflow executado por push"
```

Com essa configuração, um envio de código para qualquer branch pode disparar o workflow.

## 2. Restringindo por branch

Use `branches` para definir quais branches podem disparar a automação:

```yaml
name: Action Teste

on:
  push:
    branches:
      - main
      - 'feature/**'

jobs:
  teste:
    runs-on: ubuntu-latest
    steps:
      - name: Exibir mensagem
        run: echo "Action executada apenas em main e features"
```

Nesse exemplo, o workflow é executado em pushes para `main` e para branches que correspondam ao padrão `feature/**`, como `feature/nova-feature`.

É possível declarar vários padrões de branch. Para excluir padrões específicos, existe também `branches-ignore`; não use `branches` e `branches-ignore` juntos no mesmo evento. Escolha a forma que melhor representa a regra desejada.

### Exemplos de padrões

| Padrão | Finalidade |
|---|---|
| `main` | Somente a branch `main` |
| `feature/**` | Branches sob o padrão `feature/` |
| `release/**` | Branches sob o padrão `release/` |
| `hotfix/**` | Branches sob o padrão `hotfix/` |

**Atenção:** se o workflow estiver limitado a `main` e `feature/**`, um push para `hotfix/correcao` não o disparará.

## 3. A definição do workflow na branch atual

O GitHub Actions considera a versão do arquivo de workflow presente na branch relacionada ao evento. Por isso, se o arquivo YAML for alterado apenas em uma branch de feature, essa alteração não passa automaticamente a valer para a versão do workflow que está na `main`.

Ao testar workflows em branches diferentes, confira:
- em qual branch o commit foi feito;
- qual versão do arquivo YAML existe nessa branch;
- quais regras de `push` estão configuradas nessa versão.

## 4. Disparando por tags

Também é possível restringir o evento a tags, por exemplo, tags de versão que começam com `v`:

```yaml
name: Workflow por tag

on:
  push:
    tags:
      - 'v*'

jobs:
  teste:
    runs-on: ubuntu-latest
    steps:
      - name: Mostrar versão
        run: echo "Workflow disparado por uma tag de versão"
```

Para criar e enviar uma tag local:

```bash
git tag v1.0.0
git push origin v1.0.0
```

Para enviar todas as tags locais de uma vez:

```bash
git push origin --tags
```

A regra `tags` filtra os nomes das tags. Quando o evento `push` especifica filtros de tags, ele é disparado conforme essas regras; a configuração deve refletir se o objetivo é executar por branches ou por tags.

## 5. Filtrando por arquivos e diretórios (`paths`)

Em repositórios que contêm vários projetos ou subdiretórios, talvez não seja necessário executar uma pipeline para qualquer alteração. O filtro `paths` permite executar o workflow apenas quando os arquivos alterados corresponderem aos caminhos definidos.

Exemplo: executar somente quando houver mudanças no código dentro de `src/`:

```yaml
name: Pipeline do código

on:
  push:
    branches:
      - main
    paths:
      - 'src/**'

jobs:
  teste:
    runs-on: ubuntu-latest
    steps:
      - name: Validar alteração no código
        run: echo "O diretório src foi alterado"
```

Com essa configuração, alterações apenas no `README.md`, fora de `src/`, não atendem ao filtro de caminhos.

## 6. Ignorando caminhos (`paths-ignore`)

Use `paths-ignore` quando o workflow deve executar para alterações em geral, mas não deve executar se **todos** os arquivos modificados corresponderem aos caminhos ignorados.

Exemplo:

```yaml
name: Pipeline do código

on:
  push:
    branches:
      - main
    paths-ignore:
      - 'docs/**'
      - 'README.md'

jobs:
  teste:
    runs-on: ubuntu-latest
    steps:
      - name: Executar pipeline
        run: echo "Executando pipeline para alterações relevantes"
```

Nesse exemplo, uma alteração apenas em `docs/` ou `README.md` não dispara o workflow. Se houver alterações também em arquivos fora desses caminhos, o workflow poderá ser executado.

**Importante:** não configure `paths` e `paths-ignore` juntos para o mesmo evento `push`. Use `paths` para uma lista positiva de caminhos ou `paths-ignore` para excluir caminhos.

## 7. Combinando filtros de branch e caminho

É possível combinar filtros de branch e caminho. O workflow só será executado quando os critérios configurados forem atendidos.

```yaml
name: Pipeline do serviço

on:
  push:
    branches:
      - main
      - 'feature/**'
    paths:
      - 'src/**'
      - 'services/**'

jobs:
  teste:
    runs-on: ubuntu-latest
    steps:
      - name: Executar validação
        run: echo "Branch permitida e caminho relevante alterado"
```

Esse padrão é útil em monorepos, nos quais cada projeto pode ter sua própria pipeline. Assim, uma alteração em um serviço não precisa disparar automações de componentes sem relação com ele.

## 8. Boas práticas

- **Restrinja por branch** quando a pipeline só fizer sentido em branches específicas.
- **Use tags para versões e releases** quando o processo de publicação depender de uma tag.
- **Use `paths`** para executar tarefas apenas quando arquivos relevantes mudarem.
- **Use `paths-ignore`** para evitar execuções causadas somente por documentação ou arquivos secundários.
- **Teste as regras** em branches e commits separados, observando a aba **Actions** do repositório.
- **Mantenha os filtros simples e claros**, para facilitar a manutenção do workflow.
- **Separe automações por finalidade** quando validação de código, documentação, publicação e deploy tiverem gatilhos diferentes.

## 9. Checklist de validação

- [ ] O workflow está no diretório `.github/workflows/`.
- [ ] O evento está declarado como `on: push`.
- [ ] Os filtros de branch correspondem às branches esperadas.
- [ ] Os filtros de tags correspondem ao padrão de versionamento adotado.
- [ ] Os caminhos em `paths` ou `paths-ignore` correspondem à estrutura real do repositório.
- [ ] Foi testado um push que deve disparar o workflow.
- [ ] Foi testado um push que não deve dispará-lo.
- [ ] A versão do arquivo YAML na branch testada contém a configuração esperada.

## Resumo

O evento `push` é um dos principais gatilhos do GitHub Actions. Além de executar workflows quando código é enviado ao repositório, seus filtros permitem controlar a execução por **branch**, **tag** e **caminho de arquivo**. Configurar esses filtros adequadamente reduz execuções desnecessárias e ajuda a organizar pipelines em projetos com múltiplos componentes.

# GitHub Actions — Evento `pull_request`

## Objetivo

O evento `pull_request` permite executar workflows quando um Pull Request (PR) é aberto, atualizado ou passa por determinadas ações. Ele é útil para automatizar verificações antes de incorporar alterações a uma branch de destino, como `main`.

Entre as verificações que podem ser executadas estão:
- testes automatizados;
- análise estática e qualidade do código;
- análise de vulnerabilidades;
- validações de build;
- comentários automáticos no Pull Request.

## 1. Configuração básica

```yaml
name: Teste Pull Request

on:
  pull_request:
    branches:
      - main

jobs:
  teste:
    runs-on: ubuntu-latest
    steps:
      - name: Exibir mensagem
        run: echo "Workflow disparado por um Pull Request direcionado à main"
```

O filtro `branches` refere-se à **branch de destino** do Pull Request, não ao nome da branch de origem.

## 2. Como o fluxo funciona

1. Criar uma branch de trabalho, por exemplo, `feature/teste`.
2. Fazer alterações e enviar os commits para o repositório.
3. Abrir um Pull Request com destino à `main`.
4. O GitHub Actions executa os jobs definidos no workflow.
5. A equipe consulta os resultados antes de concluir o merge.

Se a automação identificar falhas, os resultados ficam visíveis no Pull Request. Para impedir merges com verificações reprovadas, configure as regras de proteção da branch e os checks obrigatórios nas configurações do repositório.

## 3. Validando código antes do merge

Uma pipeline de Pull Request pode executar testes, verificações de qualidade e ferramentas de segurança, como analisadores estáticos ou scanners de vulnerabilidade. Isso permite identificar problemas antes da revisão manual e reservar o Code Review humano para avaliar lógica, arquitetura, legibilidade e outros aspectos que exigem análise.

> O workflow, por si só, não reprova automaticamente um Pull Request. Para bloquear o merge com base nos resultados, configure as regras de proteção da branch e marque os checks necessários como obrigatórios.

## 4. Comentando automaticamente no Pull Request

A action `actions/github-script` permite executar JavaScript que interage com a API do GitHub, como adicionar um comentário ao Pull Request.

```yaml
name: Comentário no Pull Request

on:
  pull_request:
    types:
      - opened

permissions:
  pull-requests: write
  issues: write

jobs:
  comentar:
    runs-on: ubuntu-latest
    steps:
      - name: Adicionar comentário ao PR
        uses: actions/github-script@v7
        with:
          script: |
            const pr = context.payload.pull_request;

            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: pr.number,
              body: 'Pull Request recebido. As verificações automatizadas serão executadas.'
            });
```

O exemplo comenta quando o Pull Request é aberto. A permissão `issues: write` é necessária para criar comentários por meio desse endpoint. Conceda apenas as permissões necessárias ao workflow, evitando permissões amplas sem necessidade.

Em repositórios que recebem contribuições externas, considere as restrições de segurança aplicáveis a workflows de Pull Request.

## 5. Controlando os tipos de evento com `types`

É possível limitar o workflow a tipos específicos de atividade. Por exemplo, executar quando o PR for aberto, atualizado com novos commits ou reaberto:

```yaml
name: Verificações do Pull Request

on:
  pull_request:
    branches:
      - main
    types:
      - opened
      - synchronize
      - reopened

jobs:
  teste:
    runs-on: ubuntu-latest
    steps:
      - name: Validar alteração
        run: echo "Executando verificações do Pull Request"
```

Tipos comuns:

| Tipo | Quando ocorre |
|---|---|
| `opened` | Quando o Pull Request é aberto |
| `synchronize` | Quando a branch do Pull Request recebe novos commits |
| `reopened` | Quando um Pull Request fechado é reaberto |
| `closed` | Quando o Pull Request é fechado, com ou sem merge |

Se `types` for omitido, o GitHub utiliza os tipos de atividade padrão do evento `pull_request`. Ao declarar `types`, o workflow responde aos tipos listados.

## 6. Executando quando o Pull Request for fechado

```yaml
name: Ação após fechar PR

on:
  pull_request:
    branches:
      - main
    types:
      - closed

jobs:
  apos-fechamento:
    runs-on: ubuntu-latest
    steps:
      - name: Informar fechamento
        run: echo "O Pull Request foi fechado"
```

O evento `closed` ocorre quando o Pull Request é fechado, com ou sem merge. Se a ação deve ocorrer **somente quando houve merge**, verifique também `github.event.pull_request.merged`:

```yaml
jobs:
  apos-merge:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - name: Informar merge
        run: echo "O Pull Request foi incorporado"
```

## 7. Entendendo a branch e o contexto do evento

Para diagnosticar execuções, confira:
- qual branch foi usada como origem;
- qual branch foi escolhida como destino;
- qual tipo de atividade ocorreu;
- qual versão do arquivo de workflow está sendo utilizada;
- se os filtros correspondem à ação realizada.

Ao testar alterações no YAML, observe a branch na qual o arquivo foi alterado, pois a configuração efetivamente utilizada depende do contexto do evento e das regras do GitHub Actions.

## 8. Boas práticas

- Execute testes, análise estática e verificações de segurança antes do merge.
- Configure checks obrigatórios nas regras de proteção da branch quando necessário.
- Use `branches` para restringir Pull Requests pela branch de destino.
- Use `types` para executar apenas nas atividades relevantes.
- Declare permissões mínimas em `permissions`.
- Evite permissões amplas sem necessidade, especialmente em workflows que recebem contribuições externas.
- Separe verificações anteriores ao merge de automações posteriores ao fechamento ou merge.
- Consulte a aba **Actions** e os checks do Pull Request para investigar falhas.

## 9. Checklist de validação

- [ ] O workflow está em `.github/workflows/`.
- [ ] O evento `pull_request` está configurado.
- [ ] O filtro `branches` aponta para a branch de destino correta.
- [ ] Os tipos em `types` correspondem às atividades desejadas.
- [ ] Testes e análises relevantes são executados.
- [ ] Os checks obrigatórios estão configurados nas regras de proteção, se necessário.
- [ ] As permissões concedidas são mínimas e suficientes.
- [ ] Foram testados os cenários desejados de abertura, atualização e fechamento do Pull Request.

## Resumo

O evento `pull_request` permite automatizar validações durante o processo de revisão de código. Ele pode disparar testes, análises de qualidade e segurança ou scripts que interagem com o Pull Request. Com os filtros `branches` e `types`, permissões mínimas e regras de proteção da branch, é possível criar um fluxo de revisão mais consistente e reduzir o esforço manual da equipe.
