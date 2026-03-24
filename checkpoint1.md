# Checkpoint 1

---

## Commit 1 — Passagem de parâmetros obrigatórios na tela de Perfil

Neste commit eu implementei a passagem de um parâmetro dinâmico pela rota de navegação para a tela de Perfil. Até então, quando o usuário clicava no botão para ir ao perfil, ele era levado para uma tela genérica sem nenhuma informação personalizada. A ideia aqui foi fazer com que a rota carregue um dado — no caso o nome do usuário — e esse dado seja exibido diretamente na tela.

### O que foi alterado e por quê

**`PerfilScreen.kt` — adição do parâmetro `nome`**

A função `PerfilScreen` recebeu um novo parâmetro obrigatório chamado `nome`, do tipo `String`. Antes ela só recebia o `modifier` e o `navController`. Com essa mudança, a tela passa a exibir o nome no título, mostrando `"PERFIL - Fulano de Tal"` em vez de apenas `"PERFIL"`. Isso torna a tela consciente de quem está sendo exibido, o que é o comportamento esperado em qualquer app real.

**`MainActivity.kt` — rota com parâmetro dinâmico**

A rota da tela de Perfil foi alterada de `"perfil"` para `"perfil/{nome}"`. Essa sintaxe com chaves é a forma que o Navigation Compose usa para indicar que aquele trecho da rota é um argumento variável. Dentro do bloco `composable`, eu recupero esse argumento com `it.arguments?.getString("nome", "Usuário Genérico")`, onde `"Usuário Genérico"` funciona como valor padrão caso nenhum nome seja passado. Em seguida, o nome recuperado é enviado como argumento para a `PerfilScreen`.

O operador `!!` foi usado após a variável `nome` para dizer ao compilador que o valor não será nulo naquele ponto — já que, mesmo que o argumento não seja passado, o valor padrão garante que sempre haverá uma string.

**`MenuScreen.kt` — navegação com o parâmetro embutido na rota**

No botão que navega para o Perfil, a chamada `navController.navigate("perfil")` foi atualizada para `navController.navigate("perfil/Fulano de Tal")`. Isso significa que ao clicar no botão, o nome `"Fulano de Tal"` é passado diretamente como parte da URL de navegação, e o sistema de rotas se encarrega de extrair esse valor e entregar para a tela correta.

### Resumo do fluxo

1. O usuário está na `MenuScreen` e clica no botão de Perfil.
2. O `navController` navega para a rota `"perfil/Fulano de Tal"`.
3. O Navigation Compose reconhece o padrão `"perfil/{nome}"` e extrai o valor `"Fulano de Tal"` dos argumentos.
4. Esse valor é passado para a `PerfilScreen`, que o exibe no título da tela.
