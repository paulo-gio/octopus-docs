## Nó de Monitoramento

O validador pode verificar a situação do nó validador por meio do [Telemetry](https://telemetry.mainnet.octopus.network/).

Se for implantado automaticamente, o nome do nó validador será padronizado para `validator-[NEAR account]`, onde `[NEAR account]` é a conta NEAR usada para fazer login na Octopus Network quando o validador for registrado.

Se for implementado manualmente, o validador poderá usar a  opção  `--telemetry-url` ao executar o nó validador e definir seu valor como `'wss://telemetry.mainnet.octopus.network/submit 9'`. Além disso, o validador pode usar a opção  `--name` para definir o nome do nó validador, para que o validador possa verificar o estado do nó pelo nome do nó

### Estado do Validador

Na lista **Validators** os validadores têm principalmente o seguinte estado:

* Registrado, faltando chaves de sessão, o validador precisa definir a chave de sessão;
* Novo, chegando no dia seguinte (era), o validador precisa esperar para entrar no próximo conjunto de validador;
* Validando, o validador funciona normalmente;
* Validando, não produzindo blocos, o validador precisa verificar se o nó validador está rodando normalmente;
* Unstaking, o validador desvincula o staking;

