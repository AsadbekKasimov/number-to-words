<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Конвертер чисел в слова</title>
</head>
<body>

    <h2>Конвертер чисел в слова</h2>
    <input type="number" id="numberInput" placeholder="Введите число" min="0" max="999999999999999">
    <button onclick="convertNumber()">Преобразовать</button>
    <p id="output"></p>

    <script>
        function numberToWordsRu(num) {
            if (num === 0) return "ноль";

            const ones = ["", "один", "два", "три", "четыре", "пять", "шесть", "семь", "восемь", "девять"];
            const onesFemale = ["", "одна", "две", "три", "четыре", "пять", "шесть", "семь", "восемь", "девять"];
            const teens = ["", "одиннадцать", "двенадцать", "тринадцать", "четырнадцать", "пятнадцать",
                           "шестнадцать", "семнадцать", "восемнадцать", "девятнадцать"];
            const tens = ["", "десять", "двадцать", "тридцать", "сорок", "пятьдесят",
                          "шестьдесят", "семьдесят", "восемьдесят", "девяносто"];
            const hundreds = ["", "сто", "двести", "триста", "четыреста", "пятьсот",
                              "шестьсот", "семьсот", "восемьсот", "девятьсот"];

            const scales = [
                ["", "", ""],  // до 999
                ["тысяча", "тысячи", "тысяч"],  // 1 000 - 999 999
                ["миллион", "миллиона", "миллионов"],  // 1 000 000 - 999 999 999
                ["миллиард", "миллиарда", "миллиардов"],  // 1 000 000 000 - 999 999 999 999
                ["триллион", "триллиона", "триллионов"],  // 1 000 000 000 000 - 999 999 999 999 999
                ["квадриллион", "квадриллиона", "квадриллионов"]  // 1 000 000 000 000 000 - 999 999 999 999 999
            ];

            function getForm(n, forms) {
                if (n % 10 === 1 && n % 100 !== 11) return forms[0];
                if (n % 10 >= 2 && n % 10 <= 4 && (n % 100 < 10 || n % 100 >= 20)) return forms[1];
                return forms[2];
            }

            function convertBelowThousand(n, isFemale = false) {
                if (n === 0) return "";
                if (n < 10) return (isFemale ? onesFemale : ones)[n];
                if (n < 20 && n > 10) return teens[n - 10];
                if (n < 100) return tens[Math.floor(n / 10)] + (n % 10 !== 0 ? " " + (isFemale ? onesFemale : ones)[n % 10] : "");
                return hundreds[Math.floor(n / 100)] + (n % 100 !== 0 ? " " + convertBelowThousand(n % 100, isFemale) : "");
            }

            let result = "";
            let parts = [];
            let scaleIndex = 0;

            while (num > 0) {
                let chunk = num % 1000;
                if (chunk > 0) {
                    let isFemale = (scaleIndex === 1); // Только "тысяча" имеет женский род
                    let words = convertBelowThousand(chunk, isFemale);
                    let scaleWord = getForm(chunk, scales[scaleIndex]);

                    result = words + " " + scaleWord + (result ? " " + result : "");
                }
                num = Math.floor(num / 1000);
                scaleIndex++;
            }

            return result.trim();
        }

        function convertNumber() {
            const number = document.getElementById("numberInput").value;
            if (number === "" || isNaN(number) || number < 0 || number > 999999999999999) {
                document.getElementById("output").textContent = "Введите число от 0 до 999 999 999 999 999";
                return;
            }
            const output = numberToWordsRu(parseInt(number, 10));
            document.getElementById("output").textContent = output || "Некорректный ввод";
        }
    </script>

</body>
</html>
