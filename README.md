# game-PingPong

Jogo de Ping pong simples no qual se trabalha conceitos de variáveis, funções, movimento das raquetes com base no movimento do mouse.
Além disso estruturas de repetições de intervalos por tempo.


<html>
    <body>
        <canvas id="folha" width="600" height="500"></canvas>
        <script>

            //Carrega os primeiros comandos
            window.onload = function() {
                iniciar(); //carrega as variaveis e os primeiros comandos
                setInterval(Principal, 1000/30); //executa nosso jogo dentro do loop
            }

            //contem as variávies e os primeiros comandos
            function iniciar(){
                posicaoBolaX = posicaoBolaY = 10;
                velocidadeBolaX = 5
                velocidadeBolaY = 5
                diametroBola = 10
                posicaoJogador1 = posicaoJogador2 = 40
                pontuacaoJogador1 = 0
                pontuacaoJogador2 = 0

                folhaDesenho = document.getElementById('folha');
                areaDesenho = folhaDesenho.getContext("2d");
                larguraCampo = 600;
                alturaCampo = 500;
                espessuraRede = 5;
                espessuraRaquete = 11
                alturaRaquete = 100
                efeitoRaquete = 0.3
                velocidadeJogador2 = 7

                //função para que a barra seja controlada pelo mouse
                folhaDesenho.addEventListener('mousemove', function(e){
                posicaoJogador1 = e.clientY - alturaRaquete / 2;
                });
            }

            //colocar a bola no centro e iniciar o jogo
            function continuar(){
                posicaoBolaX = larguraCampo / 2
                posicaoBolaY = alturaCampo / 2
                velocidadeBolaX = -velocidadeBolaX
                velocidadeBolaY = 5
            }

            function desenhar(){
                 //desenho do Fundo Preto
                 areaDesenho.fillStyle = '#000000'; // cor Preta
                areaDesenho.fillRect(0,0,larguraCampo,alturaCampo);
                areaDesenho.fillStyle = '#ffffff'; // cor Branca
                //desenho da bola
                areaDesenho.fillRect(larguraCampo/2 - espessuraRede/2, 0, espessuraRede, alturaCampo); // Rede Central
                areaDesenho.fillRect(posicaoBolaX - diametroBola/2, posicaoBolaY - diametroBola/2, diametroBola, diametroBola);
                // Raquete Esquerda
                areaDesenho.fillRect(0, posicaoJogador1, espessuraRaquete, alturaRaquete)
                // Raquete Direita
                areaDesenho.fillRect(larguraCampo - espessuraRaquete, posicaoJogador2, espessuraRaquete, alturaRaquete)
                //Texto da pontuação do jogador Humano
                areaDesenho.fillText("HUMANO - " + pontuacaoJogador1 + " Pontos", 100, 100)
                //Texto da pontuação do jogador Computador
                areaDesenho.fillText("COMPUTADOR - " + pontuacaoJogador2 + " Pontos", larguraCampo - 200, 100)

            }



            //função com os calculos do jogo
            function calcular(){
                desenhar();
                //Vatiação da posição da  bola a cada 30 fps
                posicaoBolaX = posicaoBolaX + velocidadeBolaX
                posicaoBolaY = posicaoBolaY + velocidadeBolaY

                //Rebater a bola na lateral superior
                if (posicaoBolaY < 0 && velocidadeBolaY < 0){
                    velocidadeBolaY = -velocidadeBolaY
                }

                //Rebater a bola na lateral inferior
                else if (posicaoBolaY > alturaCampo && velocidadeBolaY > 0){
                    velocidadeBolaY = -velocidadeBolaY
                }

                // Condição para que o jogador pontue ou a bola reflita ao bater na raquete
                if (posicaoBolaX < 0){
                    if(posicaoBolaY > posicaoJogador1 && posicaoBolaY < posicaoJogador1 + alturaRaquete){
                    //bola Reflete
                    velocidadeBolaX = -velocidadeBolaX;

                    var diferencaY = posicaoBolaY - (posicaoJogador1 + alturaRaquete/2);
                    //Aumenta/diminue a velocidade da bola
                    velocidadeBolaY = diferencaY*efeitoRaquete;
                    }

                    else{
                        //Adiciona 1 ponto para o Computador
                        pontuacaoJogador2++;

                        //colocar a bola no centro
                        continuar();
                    }
                }
                // Condição para que o jogador pontue ou a bola reflita ao bater na raquete
                if (posicaoBolaX > larguraCampo){
                    if (posicaoBolaY > posicaoJogador2 && posicaoBolaY < posicaoJogador2 + alturaRaquete){

                        //bola Reflete
                        velocidadeBolaX = -velocidadeBolaX;

                        var diferencaY = posicaoBolaY - (posicaoJogador2 + alturaRaquete/2);
                        //Aumenta/diminue a velocidade da bola
                        velocidadeBolaY = diferencaY*efeitoRaquete;
                    }
                    else{
                        //adicionar um ponto ao Jogador Humano
                        pontuacaoJogador1++;
                        continuar();
                    }
                }

                //condicional para identificar se a bola bateu na raquete
                if ((posicaoJogador2 + alturaRaquete / 2) < posicaoBolaY){
                    //Pontuação para o Computador
                    posicaoJogador2 = posicaoJogador2 + velocidadeJogador2
                }
                else{
                    //inverter a direção da bola
                    posicaoJogador2 = posicaoJogador2 - velocidadeJogador2
                }
            }
            //executa o jogo
            function Principal(){
                desenhar()
                calcular()
            }

        </script>
    </body>
</html>
