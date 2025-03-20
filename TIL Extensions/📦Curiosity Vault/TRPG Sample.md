using System;
using System.Collections.Generic;
using System.Linq;
using System.Media;
using System.Text;
using System.Threading.Tasks;
using System.IO;
using System.Net.Http;
using System.Threading;


namespace TrpgSample
{
    public enum GameMode
    {
        None,
        Title,
        Lobby,
        Town,
        Field,
        Store,
        Inn
    }

    class Game
    {
        private GameMode mode = GameMode.Title;
        private Player player;
        private Monster monster;
        Random rand = new Random();

        public void Process()
        {
            switch (mode)
            {   // 각 장소를 구현해냄. 일부러 defalut; result; 는 안함 . 게임이 날라갈수있어서
                case GameMode.Title:
                    ProcessTitle();
                    break;
                case GameMode.Lobby:
                    ProcessLobby();
                    break;
                case GameMode.Town:
                    ProcessTown();
                    break;
                case GameMode.Field:
                    ProcessField();
                    break;
                case GameMode.Inn:
                    ProcessInn();
                    break;
            }
        }
        public void ProcessTitle()
        {
            Console.WriteLine("-----------------------------------------------------------------------------");
            Console.WriteLine("l                                                                           l");
            Console.WriteLine("l                          P R E S S                                        l");
            Console.WriteLine("l                                                                           l");
            Console.WriteLine("l                           A  N  Y                                         l");
            Console.WriteLine("l                                                                           l");
            Console.WriteLine("l                           K  E  Y                                         l");
            Console.WriteLine("l                                                                           l");
            Console.WriteLine("-----------------------------------------------------------------------------");
            string input = Console.ReadLine();
        }
        public void ProcessLobby()
        {
            Console.WriteLine("직업을 선택하세요");
            Console.WriteLine("[1] 나이트");
            Console.WriteLine("[2] 음유시인 ");
            Console.WriteLine("[3] 픽토멘서");
            Console.WriteLine("[4] 점성술사");
            string input = Console.ReadLine();
            switch (input)
            {
                case "1":
                    player = new Knight();
                    mode = GameMode.Town;
                    break;
                case "2":
                    player = new Bard();
                    mode = GameMode.Town;
                    break;
                case "3":
                    player = new Pictomancer();
                    mode = GameMode.Town;
                    break;
                case "4":
                    player = new Astrologist();
                    mode = GameMode.Town;
                    break;
            }
        }
        public void ProcessTown()
        {
            Console.WriteLine("마을에 입장하였습니다!");
            Console.WriteLine("[1] 필드로 가기");
            Console.WriteLine("[2] 상점으로 가기");
            Console.WriteLine("[3] 여관으로 가기");
            string input = Console.ReadLine();
            switch (input)
            {
                case "1":
                    mode = GameMode.Field;
                    break;
                case "2":
                    mode = GameMode.Store;
                    break;
                case "3":
                    mode = GameMode.Inn;
                    break;
            }
        }

        public void ProcessField()
        {
            Console.WriteLine("필드에 입장하였습니다.");
            Console.WriteLine("[1] 싸우기");
            Console.WriteLine("[2] 아이템 사용");
            Console.WriteLine("[3] 도망가기 ");
            string input = Console.ReadLine();
            CreateRandomMonster();
            switch (input)
            {
                case "1":
                    ProcessFight();
                    break;
                case "2":
                    useitem();
                    break;
                case "3":
                    TryEscape();
                    break;
            }
        }

        private void TryEscape()
        {
            int randomVaule = rand.Next(0, 101);
            if (randomVaule < 50)
            {
                mode = GameMode.Town;
            }
            else
            {
                ProcessFight();
            }
        }

        private void ProcessFight()
        {
            while (true)
            {
                int damage = player.GetAttack();
                monster.OnDamaged(damage);
                if(monster.isDead())
                {
                    Console.WriteLine("승리 하였습니다.");
                    Console.WriteLine($"남은 체력 {player.GetHp()}");
                    break;
                }
                damage = monster.GetAttack();
                player.OnDamaged(damage);
                if ( player.isDead())
                {
                    Console.WriteLine(" 싸늘한 시체가 되었습니다. ");
                    mode = GameMode.Title;
                    break;
                }
            }
        }

        private void CreateRandomMonster()
        {
            int randomValue = rand.Next(0, 3);
            switch (randomValue)
            {
                case 0 :
                    monster = new Slime();
                    Console.WriteLine("슬라임이 생성되었습니다.");
                    break;
                case 1:
                    monster = new Skeleton();
                    Console.WriteLine("스켈레톤이 생성되었습니다.");
                    break;
                case 2:
                    monster = new Wyvern();
                    Console.WriteLine("와이번이 생성되었습니다.");
                    break;
                
            }
        }








        public void ProcessStore()
        {

        }
        public void ProcessInn()
        { 
        
        }


    }



}


해야할거:

 // 몬스터 만들기 / 몬스터 스크립트도 짜기. (clear)
 // 도망갔을때 randomvalue 로 확률계산하기. (clear)
 // 드래곤이스터에그만들기. ( negative)
// 따로 플레이어 직업 , 몬스터 종류 스크립트 짜기 ( clear )
//game mode 스크립트 따로 만들어서 안정성 강화 ( clear)
 // process fight 도 만들어야겠다.  (clear)
 // ProcessFight 안에서 도망가기 구현 (clear)
 // 전투 대미지 , 힐 , 아이템 사용구현 (half clear)
 // CreateMonster을 통해 이또한 randvalue로 스위치문 만들기 (clear)
 // rand.next(0,5) 로 총 5마리로 만들자. (clear)
 // 드랍골드 구현 ( negative)
 // 아이템 상점 구현  ( negative)
// 독데미지도 한번 구현해보고싶다. ( negative)
// 막상 최대 hp , 현재 hp , 최대 공격력 , 기본 공격력을 설정은 해놨는데 
// 이렇게 하는게 맞나 .. ? (curious)
// 색깔 부여를 통해 직관적으로 플레이어 상태 나타내보기( negative)