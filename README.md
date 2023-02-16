package Article;

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		System.out.println("============= <게시물 관리 프로그렘> =============");
		// System.out.println("----------------- 프로그램 시작 -----------------");

		System.out.println("입력 가능한 명령들: article write, article list,\narticle remove, article detail, system exit");
		System.out.println("============================================");

		Scanner sc = new Scanner(System.in);

		int lastArticleId = 0; // 인덱싱을 위해 만든 변수
		List<Article> articles = new ArrayList<>(); // type article인 array list 만들거야. 그리고 그 array list의 이름을 articles로
													// 할거야.

		while (true) {// 무한루프 참이기 때문에 무한반복
			System.out.printf("명령어 ) ");
			String command = sc.nextLine().trim();

			if (command.length() == 0) {
				// 만약에 문자열의 크기가 0이면 아무것도 입력 안됬다는 뜻
				System.out.println("명령어를 입력해주세요");
				continue;
			}

			else if (command.equals("system exit")) {
				System.out.println("프로그렘을 종료 합니다.");
				// 프로그렘 종료를 원하면
				break;
			}

			else if (command.equals("article list")) {
				// 글 목록 보기

				if (articles.size() == 0) {
					// 만약에 articles라는 배열의 크기가 0 이면 사실상 안에 아무것도 들어있는게 아님.
					System.out.println("게시글이 없습니다");
				}

				else {
					// 만약에 articles라는 배열 안에 뭐가 들어있다면...
					System.out.println("  번호   /   제목  ");
					for (int i = articles.size() - 1; i >= 0; i--) {
						Article article = articles.get(i);
						System.out.printf("  %d    /   %s  \n", article.id, article.title);
						// 배열안에 해당하는 index에 저장된 article 라는 객체 속에 있는 객체 속성 중에 id, title 의 값들을 접근해서 출력할거야.
					}
				}
			}

			else if (command.equals("article write")) {
				// 글 추가 하기

				int id = lastArticleId + 1; // id의 초기 값은 사실상 0 + 1. 왜냐하면 lastArticleID = 0. 0+1 = 1

				// System.out.println("before" + lastArticleId);
				// System.out.println("before" + id);

				System.out.printf("제목 : ");
				String title = sc.nextLine();
				System.out.printf("내용 : ");
				String body = sc.nextLine();

				Article article = new Article(id, title, body);
				// article 이라는 변수에 new Article 객체 넣어줌.
				// 입력 할 때 마다 저장을 해야되니 서로 다른 article 객체가 articles 배열안에 있는 결과가 됨.
				articles.add(article);// articles 배열에 article 객체 추가하는 작업.

				System.out.println(id + "번 글이 생성되었습니다");
				lastArticleId++; // +1 증가시켜줌.

				// System.out.println("After" + id);
				// System.out.println("After" + lastArticleId);
			}			
			
			else if (command.startsWith("article detail ")) {
				//article 검색 기능. 입력을 받으면 article detail + (숫자가) 되니까. 문자열에서 숫자만 따로 빼와야됨. 

				String[] cmdBits = command.split(" ");
				//숫자만 따로 추출하는 방법은 split을 이용해서 하면됨.
				//현재 공백을 기준으로 문자열이 절단 되는 상황.

				int target = Integer.parseInt(cmdBits[2]);
				//절단된 문자열은 배열의 형식을 가지게 되는데 이때 필요한 정보는 마지막 조각 (숫자가 있는 index).

				Article foundArticle = null; //foundArticle 이라는 class Article 타입 변수를 만들거야. 빈 통 같은 역할.

				for (int i = 0; i < articles.size(); i++) {
					//0번째 index 에서 배열의 크기 만큼, 즉 배열의 시작점과 끝점의 index 사이의 거리를 이동할동안 
					
					
					Article article = articles.get(i);
					//articles 배열에서 각각의 인덱스마다 하니씩 순서대로 꺼내서 article 변수에 대입 할거야.
					
					
					
					if (article.id == target) {
						//articles 는 article 여러개를 담고있는 type class 배열이다. 
						//만약에 순서대로 서로 다른 article 객체 id 라는 속성의 저장된 값이 target이랑 같으면
		
						
						foundArticle = article;
						//찾았다 article. 그리고 foundArticle 변수에 대입.
						break;
						//for문 탈출
					}
				}

				if (foundArticle == null) {
					
					//만약에 찾은 article의 값이 널인 경우, 즉 일치하지 않았고 못찾았으니 못 넣은거지.
					System.out.printf("%d번 게시물은 존재하지 않습니다.\n", target);
					//못 찾았다 feedback.
					continue;
				}
				
				
				//foundArticle 찾았다면 결과 출력.

				System.out.printf("번호 : %d\n", foundArticle.id);
				System.out.printf("날짜 : 2023-12-12 12:12:12\n");
				System.out.printf("제목 : %s\n", foundArticle.title);
				System.out.printf("내용 : %s\n", foundArticle.body);

			}

			else if (command.startsWith("article remove")) {
				//articles 배열에서 원하지 않는 index의 값들 날리기.
				
				String[] cmdBits = command.split(" ");
				
				int id = Integer.parseInt((cmdBits[2]));
				
				
				articles.remove(id - 1);
				//배열은 0부터 ~
				
			}
			
			else {
				System.out.println("존재하지 않는 명령어 입니다");
			}
		}

		System.out.println("==프로그램 끝==");

		sc.close();
	}
}

class Article {
	int id;
	String title;
	String body;

	Article(int id, String title, String body) {
		this.id = id;
		this.title = title;
		this.body = body;
	}

}
