# Unity-LivingPoint
유니티하면서  잊지 말고 살아야할것을 적어 놓습니다

## 개요

유니티를 하루종일 만지고 살다보면 늘 분노에 차 있습니다.
다시는 같은 실수를 반복하지 않기 위해 메모를 해 두도록 합시다.

## XCode를 믿지마라

하루는 왜이렇게 빌드가 오래걸리나 목빠지게 기다린적이 있습니다.

근데 묘한 느낌이 들어 빌드과정을 매번 쳐다보고 기억하다보니, 무언가 위화감을 느꼈습니다.

처음 분명했을땐 분명 260개의 소스를 컴파일하던 반면, 어느새 520개를 컴파일 하고 있는게 아닙니까?

아차, 하는 느낌이 들어 [Build And Run]이 아니라 [Build]를 선택하고 Append가 아닌 Replace로 빌드하고 보니 260개로 돌아왔습니다.
옳구나. 기본으로 Append가 설정되고, 파일이 중복되어 이녀석이 2배로 빌드하고 있던것이였습니다.

빌드시간을 단축하고 싶다면... 가급적 Unity Clound Build를 사용하거나, 혹여 그럴 환경이 안된다면 손 수 Build를 눌러 빌드해줍시다.
사이닝이나 빌드 설정을 그때그때 다시 지정해줘야 하지만 시간을 절약하기 위해 이정도 귀찮음은 감수해줍시다.

## 에셋스토어, 양날의 검

유니티 에셋스토어에서 완성된 프로젝트에서 개조하여 진행하는 식으로 시간을 아끼려 했으나...

비효율 적인 최적화, 최악의 씬구조, 떨어지는 코드 재사용성... 불편하기 짝이 없습니다 이럴거면 처음부터 짤걸 하는 생각도 들었습니다

남들이 만든걸 쓰는건 양날의 검이라는 사실을 잊지 맙시다

오픈 소스는 공짜니까 다들 고쳐서 풀리퀘라도 해놓지만 돈주고 사는 에셋스토어는 아니랍니다

## 싸고 좋은 물건엔 흠이 있기 나름이다

뭔가 굉장한 에셋을 발견했다 싶은 느낌이 들어 적용하려고 보면 항상 약관, 사후지원, 혹은 쉬운 충돌이나 구현도의 부족...

많은 장벽에 시달립니다

정말 필요한거라면 만들어쓰고, 에셋에 의존하는건 리소스에 만족하도록 합시다

## iCloud가 만들어낸 대 참사

일반적으로 Mac을 사용하는 개발자라면, iClound를 사용하고 있을겁니다

iCloud는 자주 사용하지 않는 파일을 클라우드에 백업하고, 필요할때 다운 받는 식으로 저장공간 최적화를 하는 기능이 있는데

아차, 프로젝트 파일도 클라우드에 올려버리고 로컬에서 지워버리곤 하는데, 파인더에는 정상적으로 보이니 알 길이 없습니다

꼭 엥? 코드도 정상인데 왜 이러지? 왜 이 컴포넌트가 Missing됐지? 하고 찾아보면 꼭 이녀석이 말썽입니다

프로젝트 폴더는 따로 빼두도록 합시다

## 과한 친절은 사양할게요

에셋을 받아서 쓰다보면, 꼭 원본 리소스 파일, 데모 씬, 샘플이나 관련 문서가 포함되어 있는 경우가 많습니다

친절은 좋지만 문제는 용량이 너무 크다는거죠

귀찮다고 내버려 두고 살다가는 깃 용량제한에 걸려 금방 골치아파지곤 합니다

반드시 정리하거나 임포트시 제외하도록 합시다

## 프로젝트 이동에는 Assets/랑 ProjectSettings/만 있으면 돼요

꼭 git에 Temp랑 Objs, Library까지 다 올리시는 분들이 계십니다

그러면 풀받을때, 클론받을때, 너무 오래걸리고 용량도 금방 차요

꼭 다음과 같은 내용의 .gitignore를 첨부해줍시다
```
[Ll]ibrary/
[Tt]emp/
[Oo]bj/
[Bb]uild/
[Bb]uilds/
Assets/AssetStoreTools*

# Visual Studio cache directory
/.vs/

# Autogenerated VS/MD/Consulo solution and project files
ExportedObj/
.consulo/
*.csproj
*.unityproj
*.sln
*.suo
*.tmp
*.user
*.userprefs
*.pidb
*.booproj
*.svd
*.pdb

# Unity3D generated meta files
*.pidb.meta
*.pdb.meta

# Unity3D Generated File On Crash Reports
sysinfo.txt

# Builds
*.apk
*.unitypackage  
```

## C#이지만 C#이 아니다

알다시피 유니티는 옛날옛적 C#, Mono 2.x대를 사용하고 있고 닷넷 또한 사정이 그렇습니다...

즉 async await 같은 동기 프로그래밍 고오오오급 기술은 진작에 포기하는게 좋습니다

요즘들어 버전도 조금씩 올라가고, 플레이어 세팅에서 버전 설정도 가능하길래 써봤는데요

어... 음... 중복 정의 관련해서 터지더라구요

정신건강을 위해 Task나 delegate, event를 활용한 콜백을 사용하거나

UniRx를 공부해보시는걸 권장드립니다.

## 씬로딩은 왜 막았니...?

PUN(Photon Unity Network)를 사용하고 있다면

PhotoClasses.cs파일을 한번 확인해보자.

```C#
namespace UnityEngine.SceneManagement
{
    /// <summary>Minimal implementation of the SceneManager for older Unity, up to v5.2.</summary>
    public class SceneManager
    {
        public static void LoadScene(string name)
        {
            Application.LoadLevel(name);
        }

        public static void LoadScene(int buildIndex)
        {
			
            Application.LoadLevel(buildIndex);
        }
    }
}
```

세상에, 씬매니저를 갈아 치워서 다른 기능을 사용할 수 없게 중복정의된 상태다.
```#if !UNITY_2017_1_OR_NEWER```로 감싸서 복구해주도록 하자.
그럼 CreateScene, LoadSceneAsync등을 사용할 수 있게 된다.
