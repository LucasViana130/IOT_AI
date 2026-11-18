# Evidências da entrega de IoT e IA

Coleta realizada em 18/11/2026, executando o projeto Java na máquina do grupo.
O commit testado e os resultados por caso estão em [testes-java.json](testes-java.json).

A execução concluiu com BUILD SUCCESS, 21 suítes, 182 testes descobertos,
181 aprovados, um teste manual Gemini ignorado, zero falhas e zero erros.
Health respondeu 200 com UP. Swagger e OpenAPI responderam 200, com rota
de guidance e autenticação Bearer JWT presentes no contrato.

| Registro | O que demonstra |
| --- | --- |
| [Testes Java](testes-java.json) | Casos efetivamente executados, aprovados, ignorados ou com falha, agrupados por suíte |
| [Disponibilidade HTTP](disponibilidade-api.json) | Respostas observadas em health, OpenAPI e Swagger no momento da coleta |
| [Gemini real em 17/11](gemini-live-2026-11-17.json) | Registro histórico de geração com fallback=false, quatro fontes e risco preservado |
| [Tentativa real em 18/11](gemini-tentativa-2026-11-18.json) | A tentativa atual recebeu fallback PROVIDER_UNAVAILABLE e falhou no critério de geração real |

## Execução local

Foi utilizado OpenJDK Temurin 21.0.9 e Apache Maven 3.9.16 no Windows, com Docker Desktop.
Comando executado na raiz do GS-FINAL, commit 9c0165f80282becda19ba0f27c82d9c9344c825e

```sh
mvn -B verify
```

Os testes de integração iniciam PostgreSQL/PGVector e RabbitMQ descartáveis por Testcontainers.
Os clientes de IA dos testes comuns são controlados. O teste manual Gemini exige habilitação
e credencial própria. O resultado exato da execução está no JSON, inclusive casos ignorados.

O exportador conserva nomes e resultados dos testes. Propriedades de ambiente, saída bruta
de processos, tokens e credenciais ficam fora dos relatórios públicos.

Para reproduzir a coleta depois de executar os testes, a partir deste repositório

```sh
python evidencias/coletar_relatorios.py /caminho/para/GS-FINAL
```

Consulte os testes citados pelo nome dentro de src/test/java no GS-FINAL. JobGuidanceTests,
RiskJobGuidanceIntegrationTests, RagAndPromptTests e AiConfigurationTests verificam
orientação, persistência, isolamento, recuperação, prompt e configuração. WaterIndicatorsTests
e WaterRiskCalculatorTests cobrem regras determinísticas. ReadingRabbitIntegrationTests
verifica a mensageria real em ambiente descartável.

## Provedor real

O registro de sucesso de 17/11 foi copiado sem alteração de
https://github.com/LucasViana130/agro-conecta-iot-ai/blob/d6b52beeca6efdda2f285bc86d994ab33e8cf5a0/docs/evidencias/gemini-live.json

Ele conserva a data original e o commit informado naquela execução. Esse registro omite
o texto integral da orientação e contém as fontes e as asserções resumidas do roteiro.

Em 18/11 o script scripts/verify_live_ai.py do mesmo repositório foi executado contra
a API no Render. Ele criou recursos DEMO com credenciais aleatórias apenas em memória,
enviou leituras, aguardou o resultado e pediu orientação. O servidor retornou
PROVIDER_UNAVAILABLE. O processo terminou com código 1 no critério que exige geração válida.
A investigação da causa interna permanece pendente. Esse resultado precisa ser considerado
na preparação da demonstração com o professor.

Os testes locais verificam contratos e tratamento de falhas. A revisão agronômica do texto,
a calibração de sensores físicos e a avaliação de impacto com produtores continuam pendentes.
