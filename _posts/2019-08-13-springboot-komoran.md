---

layout: post

title: Komoran 한국어 형태소 분석기 적용

date: 2019. 08. 13

---

Spring Boot / Gradle 기반 서비스에 Komoran 한국어 형태소 분석기 적용

#### # Komoran
Java로 구현된 한국어 형태소 분석기이다.
Jar 또는 Maven/Gradle 형태로 빌드 후 바로 사용이 가능하다.
외부 의존성이 없어서 설치 후 바로 사용 가능하기에 간단한 서비스에 적용해 사용하기에 최적인 것 같다.

* [Komoran 공식 사이트](https://www.shineware.co.kr/products/komoran/)
* [Komoran Githiub](https://github.com/shin285/KOMORAN)
* [Komoran 설치 가이드](https://docs.komoran.kr/firststep/installation.html?utm_source=komoran-repo&utm_medium=Referral&utm_campaign=github-demo)
* [Komoran 품사표](https://docs.komoran.kr/firststep/postypes.html)
- - -


#### # Gradle build.gradle 설정
* Komoran 3.3.4 버전 설정
```[java]

	repositories {
		maven { url 'https://jitpack.io' }
	}
	dependencies {
		implementation 'com.github.shin285:KOMORAN:3.3.4'
	}
```

- - -

#### # Komoran Util
* Singleton Pattern Util 생성 후 사용
 > 여러 곳에서 사용해야 하는 경우가 발생해 싱글톤 패턴으로 Util Class 생성
 > 형태소 분석 사전 등록 및 관리 편하게 하기 위해...

 ```[java]
	// Komoran 형태소 분석 사용 Util
	public class KomoranUtils {
		private static final Logger log = LoggerFactory.getLogger(KomoranUtils.class);
		private KomoranUtils(){}

		public static Komoran getInstance() {
			return KomoranInstance.instance;
		}

		private static class KomoranInstance {
			public static final Komoran instance = new Komoran(DEFAULT_MODEL.FULL);
		}
	}
 ```

- - -

#### # Komoran 테스트

 * Spring Boot 테스트 진행
 ```[java]
	@SpringBootTest
	@ActiveProfiles("local-live")
	@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)    // DB 사용 설정
	@RunWith(SpringRunner.class)
	public class KomoranTest {

		private static final Logger log = LoggerFactory.getLogger(KomoranTest.class);

		@Test
		public void KomoranUtilSingletonTest() {
			List<String> analyzeList = KomoranUtils.analyzeKeywordList("광주세계선수권대회");
			for (String title : analyzeList) {
				log.info(title);
			}
		}

		@Test
		public void Test() {
			Komoran komoran = KomoranUtils.getInstance();
			KomoranResult analyzeResultList = komoran.analyze("부산코미디페스티벌NAVER");

			List<Token> tokenList = analyzeResultList.getTokenList();

			// 1. print each tokens by getTokenList()
			log.info("==========print 'getTokenList()'==========");
			for (Token token : tokenList) {
				log.info(token.toString());
				log.info(token.getMorph()+"/"+token.getPos()+"("+token.getBeginIndex()+","+token.getEndIndex()+")");

			}

			// 2. print nouns
			log.info("==========print 'getNouns()'==========");
			log.info(analyzeResultList.getNouns().toString());

			// 3. print analyzed result as pos-tagged text
			log.info("==========print 'getPlainText()'==========");
			log.info(analyzeResultList.getPlainText());

			// 4. print analyzed result as list
			log.info("==========print 'getList()'==========");
			log.info(analyzeResultList.getList().toString());

			// 5. print morphes with selected pos
			log.info("==========print 'getMorphesByTags()'==========");
			log.info(analyzeResultList.getMorphesByTags("NNG", "NNP", "NNB", "SL").toString());
		}
	}
 ```
 * 실행 결과
```[java]
	// KomoranUtilSingletonTest 테스트
	20190813 17:10:48.984 [main] INFO c.s.s.v.KomoranTest - 광주 
	20190813 17:10:48.984 [main] INFO c.s.s.v.KomoranTest - 세계 
	20190813 17:10:48.984 [main] INFO c.s.s.v.KomoranTest - 선수권대회 
	
	// Test 테스트
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - ==========print 'getTokenList()'========== 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - Token [morph=부산, pos=NNP, beginIndex=0, endIndex=2] 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - 부산/NNP(0,2) 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - Token [morph=코미디, pos=NNG, beginIndex=2, endIndex=5] 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - 코미디/NNG(2,5) 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - Token [morph=페스티벌, pos=NNP, beginIndex=5, endIndex=9] 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - 페스티벌/NNP(5,9) 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - Token [morph=NAVER, pos=SL, beginIndex=9, endIndex=14] 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - NAVER/SL(9,14) 
	20190813 17:10:48.971 [main] INFO c.s.s.v.KomoranTest - ==========print 'getNouns()'========== 
	20190813 17:10:48.972 [main] INFO c.s.s.v.KomoranTest - [부산, 코미디, 페스티벌] 
	20190813 17:10:48.972 [main] INFO c.s.s.v.KomoranTest - ==========print 'getPlainText()'========== 
	20190813 17:10:48.972 [main] INFO c.s.s.v.KomoranTest - 부산/NNP 코미디/NNG 페스티벌/NNP NAVER/SL 
	20190813 17:10:48.972 [main] INFO c.s.s.v.KomoranTest - ==========print 'getList()'========== 
	20190813 17:10:48.972 [main] INFO c.s.s.v.KomoranTest - [Pair [first=부산, second=NNP], Pair [first=코미디, second=NNG], Pair [first=페스티벌, second=NNP], Pair [first=NAVER, second=SL]] 
	20190813 17:10:48.972 [main] INFO c.s.s.v.KomoranTest - ==========print 'getMorphesByTags()'========== 
	20190813 17:10:48.972 [main] INFO c.s.s.v.KomoranTest - [부산, 코미디, 페스티벌, NAVER] 
```

- - -

#### # Komoran 사용기
 > 한국어 자연어 형태소 분석기를 찾던중 설치 방법과 사용법이 간단해 바로 적용해 봤다.
 > 결과는 대만족 이었다.
 > 아주 간단한 내용이기 때문에.. 여기에 아주 적격이었다.
 > 하지만, 특정 키워드 분석 및 실시간 처리 속도는 다른 형태소 분석기에 살짝 부족한면이 있는것 같다.
 > Tomcat 서버에 min:256, max:512 설정 후 순간 처리 속도를 테스트 하다 보니 out of memory 놨다.
 > 실시간 검색 키워드 처리는 살짝 무리일듯~ 그냥 엘라스틱서치를 사용하는게 좋을것 같다.

- - -







