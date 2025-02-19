## Olá 👋
[English version](ENGLISH.md) <br>
Oi, meu nome é renan é aqui estão algumas informações sobre mim

### Linguas de programação e tecnologias que eu uso:
- Lua (Majoritariamente para jogos, porem uso luvit para algumas ferramentas em linha de comando) 
- C# (Winforms)
- JS (Um pouco de front-end e um pouco de Node.js)
- PHP e mySql
- C++ (basico)

### Eu planejo aprender:
- TypeScript
- Julia
- React e React Native
- GD Script
- GLSL
- C
- C++ (avançado)

### Specs do meu PC:
- Dell inspiron 3421
- I3 3217u 1.8GHZ
- 1TB HDD
- 12 GB DDR3 RAM

<!--
**Laggh/Laggh** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

<?php
function generateContent(string $api_key, string $prompt): ?string
{
    $url =
        "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=" .
        $api_key;


    $data = [
        "contents" => [
            [
                "parts" => [
                    [
                        "text" => $prompt,
                    ],
                ],
            ],
        ],
    ];

    $options = [
        "http" => [
            "method" => "POST",
            "header" => "Content-Type: application/json\r\n",
            "content" => json_encode($data),
        ],
    ];

    $context = stream_context_create($options);
    $result = @file_get_contents($url, false, $context);  // Suppress warnings

    if ($result === false) {
        $error = error_get_last();
        return null; // Or handle the error more specifically
    }

    $decoded_result = json_decode($result, true);

    if (
        $decoded_result &&
        isset($decoded_result["candidates"]) &&
        count($decoded_result["candidates"]) > 0 &&
        isset($decoded_result["candidates"][0]["content"]["parts"][0]["text"])
    ) {
        // return json_encode($decoded_result, JSON_PRETTY_PRINT);
        return $decoded_result["candidates"][0]["content"]["parts"][0]["text"];
    } else {
        return null; // Or handle the unexpected response format
    }
}

$tema = "aleatorio";

if(isset($_GET['tema'])){
    $tema = $_GET['tema'];
}

$prompt = "Você é um servidor http, responda isso com apenas o html de uma pagina da web com o tema: { $tema }, use css inline sem imagens e se tiver botões ou hyperlink, eles devem redirecionar a mesma pagina porem com um GET no link do tema exemplo:
<btn> Jogos de ação </btn> --> ?tema=sobre-o-tema-jogos-de-acao
<a> Free fire </a> --> ?tema=free-fire
retorne o html completo com a tag html, head e body";


$str = generateContent("AIzaSyBphdUJGPYIplSaY816aLoOzdyrLuz93pU", $prompt);

$str = substr($str, 7, -4); //Remover o ```html do inicio e do final
echo $str;
?>
