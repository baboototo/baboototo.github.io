---

layout: post

title: Open Korean Text 한국어 형태소 분석기 적용

date: 2019. 08. 14

---

Spring Boot / Gradle 기반 서비스에 Open Korean Text 한국어 형태소 분석기 적용

#### # Open Korean Text
트위터에서 개발된 오픈소스 한국어 처리기이지만, 이제는 Open Korean Text 오픈소스로 변경되었다.
스칼라로 쓰인 Java 한국어 처리 라이브러리이며, 텍스트 정규화 형태소 분석기이다.


* [twitter-korean-text Github](https://github.com/twitter/twitter-korean-text)
* [Open Korean Text Github](https://github.com/open-korean-text/open-korean-text)


- - - -

#### # 라이브러리 다운로드 및 라이브러리 적용
* [Open Korean Text 라이브러리 다운로드](https://jar-download.com/artifacts/org.openkoreantext/open-korean-text/2.1.0/source-code)
 > 파일 다운로드 및 압축 해제
 > open-korean-text-2.1.0.jar 파일을 프로젝트 라이브러리 폴더에 복사

- - - -

#### # Gradle build.gradle 설정
* Scala 라이브러리를 추가
* 트위터 한국어 라이브러리 추가
 > Open Korean Text 라이브러리 추가 시 트위터 라이브러리가 없을 경우 오류 남.

 ```[java]

	repositories {
		maven { url 'https://jitpack.io' }
	}
	dependencies {
		implementation files('lib/open-korean-text-2.1.0.jar')

		implementation 'org.scala-lang:scala-library:2.12.4'
    	implementation 'com.twitter.penguin:korean-text:4.4.4'
	}
 ```

- - -

#### # com.twitter.penguin:korean-text 미적용시 발생하는 오류
 > Gradle build.gradle 설정에 com.twitter.penguin:korean-text를 미적용 하니 아래와 같은 문제가 발생했다.
 > open-korean-text 라이브러리는 트위터 라이브러리 의존성을 가지고 있는듯 하다.

![](http://baboototo.github.io/images/blogs/springboot/springboot-okt.png)


- - -

#### # Open Korean Text 테스트

 * Spring Boot 테스트 진행
 ```[java]
	@SpringBootTest
	@ActiveProfiles("local-live")
	@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)    // DB 사용 설정
	@RunWith(SpringRunner.class)
	public class OpenKoreanTextTest {

		private static final Logger log = LoggerFactory.getLogger(OpenKoreanTextTest.class);

		@Test
		public void Test() {

			String text = "BTS방탄소년단야동#방소 ";

			// Normalize
			CharSequence normalized = OpenKoreanTextProcessorJava.normalize(text);
			System.out.println(normalized);

			// Tokenize
			Seq<KoreanTokenizer.KoreanToken> tokens = OpenKoreanTextProcessorJava.tokenize(normalized);
			System.out.println(OpenKoreanTextProcessorJava.tokensToJavaStringList(tokens));
			System.out.println(OpenKoreanTextProcessorJava.tokensToJavaKoreanTokenList(tokens));

			// Phrase extraction
			List<KoreanPhraseExtractor.KoreanPhrase> phrases = OpenKoreanTextProcessorJava.extractPhrases(tokens, true, true);
			System.out.println(phrases);

			phrases = OpenKoreanTextProcessorJava.extractPhrases(tokens, false, true);
			System.out.println(phrases);

			phrases = OpenKoreanTextProcessorJava.extractPhrases(tokens, true, false);
			System.out.println(phrases);
		}
	}

 ```
 * 실행 결과
```[java]
	// OpenKoreanTextTest 테스트
	BTS방탄소년단야동#방소
	[BTS, 방탄소년단, 야동, #, 방소]
	[BTS(Alpha: 0, 3), 방탄소년단(Noun: 3, 5), 야동(Noun: 8, 2), #(Punctuation: 10, 1), 방소(Noun: 11, 2)]
	[방소(Noun: 11, 2), BTS(Noun: 0, 3), 방탄소년단(Noun: 3, 5)]
	[BTS방탄소년단야동(Noun: 0, 10), 방소(Noun: 11, 2), BTS(Noun: 0, 3), 방탄소년단(Noun: 3, 5), 야동(Noun: 8, 2)]
	[방소(Noun: 11, 2), BTS(Noun: 0, 3), 방탄소년단(Noun: 3, 5)]
```

- - -

#### # Open Korean Text 적용기
데이터 분석 설계하는 곳에서 open-korean-text 사용하기에 나도 동일한 OKT(Open Korean Text) 적용했다.
Java 라이브로리가 있다고 해서 적용을 해보니 Gradle 등록된 라이브러리가 아니였으며, 또 Scala 개발된 라이브러리 였다.
적용하기 힘들었다. 힘들다기 보다는 다 찾아 가는것이 살짝 귀찮아 졌다.
직접 라이브러리를 찾아 다운로드 후 프로젝트에 적용하고 스칼라 라이브러리 등록 하고 테스트 했더니
트위터 라이브러리가 또 필요해 졌다. 트위터에서 파생되 나온 라이브러리인지 꼭 필요했다. 그래서 모두 적용했다.
적용 후 테스트 실행을 해보니 나름 만족했다.
트위터에서 개발된 것이라 그런지 검색 제외 필터 기능과 해시태그 분석이 가능해서 좋핬다.
지금 Komoran과 OKR(Open Korean Text) 두개 모두 적용후 테스트 중에 있다.
두 기능을 모두 장단점을 파악 하기 위해 테스트 진행중에 있다. 하나를 선택 해야 한다. ㅎㅎㅎ
관리 측면에서 계속 발생하는 한글 자연어 형태소 분석을 진행하게 될것 같은데
사전등록 / 제어 단어 필터 / 실시간 처리는 어떻게 해야 할지 많은 생각이 든다.
자연어 형태소 분석을 하면서 우리는 많은 문장과 단어를 축약해서 사용하고 있다는걸 느끼게 됐다.
축약된 단어를 사용하기는 편하지만 그 단어의 순수 의미를 잃어버리는것 같아 아쉬움이 남는다.
참고로 난 엄마 라는 단어가 참 좋은데~ 왜? 다들 맘 이라고 부르는지 살짝 이해가 안되네 ㅎㅎㅎㅎ


- - -







