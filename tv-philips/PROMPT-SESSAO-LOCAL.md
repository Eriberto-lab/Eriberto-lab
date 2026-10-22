# Prompt para a sessão do Claude no computador

Copie tudo abaixo da linha e cole na sessão do Claude rodando no computador
(Claude Desktop ou `claude remote-control`), que está na mesma rede Wi-Fi da TV.

---

Oi! Quero que você melhore minha TV Philips (Android TV ou Google TV, não sei
qual dos dois) pelo Wi-Fi, usando ADB a partir deste computador. Não manjo de
configuração, então faça o máximo possível sozinho e me peça só o que precisar
ser feito na TV com o controle. Fale comigo em português, de forma simples.

## Objetivo
1. Trocar o launcher por um mais bonito e leve (preferência: **Projectivy Launcher**).
2. Deixar a TV mais rápida (animações mais curtas, menos coisa rodando em segundo plano).
3. Desativar apps de fábrica inúteis, **sempre me perguntando antes**.

## O que eu já fiz (ou vou fazer) na TV
- Ativei o modo desenvolvedor (cliquei 7x em "Build" em Configurações → Sobre).
- Liguei a **Depuração USB/ADB** em Opções do desenvolvedor.
- Se não tiver feito, me guie passo a passo, dizendo os nomes dos menus em português.

## Passo a passo sugerido
1. **Preparar o computador:** veja se o `adb` está instalado. Se não estiver, instale
   o Android Platform Tools pelo método mais simples deste sistema operacional
   (winget/brew/apt ou download oficial do Google).
2. **Achar a TV na rede:** pergunte o IP (Configurações → Rede → Status) ou descubra
   sozinho (`arp -a`, varredura da porta 5555 na sub-rede local). Depois rode
   `adb connect IP:5555` e me avise para aceitar o aviso na TV ("Sempre permitir" → OK).
3. **Diagnóstico:** antes de mudar qualquer coisa, colete e me mostre um resumo:
   - modelo (`getprop ro.product.model`), versão do Android e se é Google TV ou Android TV;
   - RAM livre e armazenamento (`dumpsys meminfo`, `df`);
   - launcher atual e lista de pacotes (`pm list packages`).
4. **Backup:** salve neste computador, numa pasta `tv-philips-backup/`, a lista de pacotes
   e as configurações atuais de animação, para dar pra desfazer tudo depois.
5. **Instalar o Projectivy Launcher:**
   - Opção A: baixe o APK mais recente dos releases oficiais
     (https://github.com/spocky/miproja1/releases) e rode `adb install`.
   - Opção B: abra a página da Play Store na TV
     (`adb shell am start -a android.intent.action.VIEW -d "market://details?id=com.spocky.projengmenu"`)
     e me peça para clicar em Instalar.
   - No Google TV não dá pra simplesmente trocar o launcher padrão. O Projectivy usa
     a opção "Substituir botão Home" (serviço de acessibilidade). Ative isso pra mim
     via ADB se der; se não der, me guie nos menus.
6. **Velocidade:**
   - `settings put global window_animation_scale 0.5`,
     `transition_animation_scale 0.5` e `animator_duration_scale 0.5`.
     Pergunte se eu prefiro 0.5 ou desligado.
   - Liste os apps que mais consomem memória ou rodam em segundo plano.
7. **Apps de fábrica:** monte uma tabela com os candidatos a desativar (nome amigável,
   pacote, o que faz, risco) e só desative o que eu aprovar, usando
   `pm disable-user --user 0 <pacote>` (reversível com `pm enable`).
   **Nunca** desinstale apps de sistema e **nunca** desative: Play Store, Google Play
   Services, Configurações, entradas HDMI/TV, Ambilight, teclado, controle remoto,
   atualizações do sistema, nem o launcher original (deixe como reserva).
8. **Final:** reinicie a TV (`adb reboot`), confirme que tudo funciona e me entregue
   um resumo do que mudou e o comando para desfazer cada coisa. Salve esse resumo em
   `tv-philips-backup/RESUMO.md`.

## Regras
- Explique o que vai fazer antes de cada etapa que muda alguma coisa na TV.
- Se algo der erro, pare e me explique em linguagem simples.
- Se eu precisar mexer na TV, me diga exatamente o que aparece na tela e onde clicar.
  Posso mandar foto da tela se ajudar.

## Alternativas de launcher (se o Projectivy não agradar)
- **AT4K Launcher:** visual estilo Apple TV, leve, com widgets.
- **LtvLauncher:** código aberto, fork atualizado do FLauncher
  (https://github.com/LeanBitLab/LtvLauncher).
