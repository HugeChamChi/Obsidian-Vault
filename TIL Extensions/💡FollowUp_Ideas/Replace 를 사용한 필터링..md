
게임내 비속어 필터링 기능 구현 

```cs
public class BadWord {
    // 비속어 리스트
    private static readonly List<string> badWords = new List<string> {
        "야발","야랄" // 필터링할 비속어 추가
    };

    public static string FilterBadWords(string input) {
        foreach (var badWord in badWords) {
            // 비속어를 "****"로 대체
            input = input.Replace(badWord, new string('*', badWord.Length));
        }
        return input;
    }

    public static void Main(string[] args) {
        Console.WriteLine("입력할 문장을 작성하세요:");
        string userInput = Console.ReadLine();

        string filteredText = FilterBadWords(userInput);
        Console.WriteLine($"필터링된 결과: {filteredText}");
    }
}
```
