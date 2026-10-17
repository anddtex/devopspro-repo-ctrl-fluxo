# devopspro-repo-ctrl-fluxo
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
