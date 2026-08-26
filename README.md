1. **O que representa a etapa de CI neste projeto?**
   A etapa de **Continuous Integration (CI)** é representada pelo job `ci`. Ela baixa o código, configura o Python 3.13, atualiza o `pip`, instala as dependências do `requirements.txt` e executa os testes automatizados com `pytest -v`. Assim, ela verifica se o código está funcionando corretamente antes de continuar o pipeline

2. **O que impede a execução do Continuous Delivery quando existe um defeito?**
   O `delivery` possui a configuração `needs: ci`. Isso significa que ele **depende da conclusão bem-sucedida do job `ci`**. Se os testes do `pytest` falharem, o job `ci` falha e, consequentemente, o `delivery` não é executado

   Além disso, o `delivery` só é executado quando o evento é um `push` na branch `main`, devido à condição:

   `if: github.event_name == 'push' && github.ref == 'refs/heads/main'`

3. **Qual seria a próxima etapa necessária para transformar este pipeline em Continuous Deployment?**
   Seria necessário adicionar uma etapa de **Continuous Deployment (CD)** após o `delivery`, responsável por **implantar automaticamente o artefato gerado em um ambiente de produção**

   Atualmente, o pipeline apenas cria o pacote em `dist/` e o publica como artefato usando `actions/upload-artifact@v7`. Para ser Continuous Deployment, seria necessário acrescentar um job que, após o `delivery`, faça o **deploy automático da aplicação para um servidor ou serviço de hospedagem**
