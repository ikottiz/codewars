## My solution (27/31 кейсов, ≈87.1%)

[Link](https://www.codewars.com/kata/decode-the-morse-code-for-real "Link")
Я решал этот набор задач в формате **6 kuy -> 4 kuy -> 2 kuy** за 2 часа, в этом(2 kuy) задании нужно использовать **k-means кластеризацию**, а не медианы так как разбиение по длительности `1` и `0` не всегда точное, особенно на длинных последовательностях, я сдался на 85% точности, потому что для прохождения дальше моих теоретических навыков **не хватает**
[Видео которое может помочь решить данную задачу](https://www.youtube.com/watch?v=8vCuR1AndH0 "youtube")
### Код:

```python
import re
import statistics

def decodeBitsAdvanced(bits):
    clean_bits = bits.strip("0")
    if not clean_bits:
        return ""

    lengths = []
    previous = "0"
    start_index = 0

    for index in range(len(clean_bits)):
        if clean_bits[index] != previous:
            lengths.append(index - start_index)
            start_index = index
            previous = clean_bits[index]

    lengths.append(len(clean_bits) - start_index)
    lengths = [l for l in lengths if l > 0]

    min_unit = statistics.median(lengths) if lengths else 1

    chunks = re.findall(r'1+|0+', clean_bits)
    result = ""

    for chunk in chunks:
        length = len(chunk)

        if chunk[0] == "1":
            if length <= min_unit * 1.5:
                result += "."
            else:
                result += "-"
        else:
            if length <= min_unit * 1.5:
                result += ""
            elif length <= min_unit * 3.5:
                result += " "
            else:
                result += "   "

    return result

def decodeMorse(morse_code):
    words = morse_code.strip().split('   ')
    decoded_words = []

    for word in words:
        letters = word.split(' ')
        decoded_word = ''.join(MORSE_CODE.get(letter, '') for letter in letters)
        decoded_words.append(decoded_word)

    return ' '.join(decoded_words) 
```
