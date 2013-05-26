Bubble-shoots
======

                                                           
          Bubble shoot


Семинарската, чие име е “Bubble shoоt”, претставува игра каде се чека на корисникот да ги успука балончиња чие движење започнува од рандом дефинирана позиција, по определена траекторија. Балочињата треба да бидат испукани пред истекувањето на времето. Играта содржи три нивоа. Во зависност од нивото кое е избрано балончињата ги менуваат своите карактеристики. На најлесното ниво, наречено light, балончињата се најголеми и најмалку на број, а во наредните две нивоа бројот на балончиња се зголемува, а се намалува нивната дименизја соодветно.
Корисникот уште при стартување на играта има право да избере кое ниво сака да го игра.
  
За да ја почнете играта потребно е да внесете име.  По внесувањето на името избирате соодветно ниво кое сакате да го избирате,и со тоа се стартува вашата игра . 

 

Формата во позадина е поделена на три дела. Секој од трите дела имаат соодветно различни граници. Во горниот дел, кој зафаќа половина од формата, се движат балоните, на средина се движи човечето кое треба да ги испука балоните, а на дното е ставен ProgressBar, во кој се кажува времето кое ви преостанува за играта и рамнина на која стои човечето. Истата рамнина ја менува својата боја како што се намалува времето во прогрес барот.
Класа man:
Класата man, во која се оформува човечето кое се движи и ги пука балоните ги има следните аргументи
public enum DIRECTION
        {
            RIGHT,
            LEFT,
        }

        public int X { get; set; }
        public int Y { get; set; }
        public Image coveche;
        public DIRECTION Direction;
        public Rectangle Bounds;

За формираното човече дефинирана е и можност истото да се движи преку стрелките на тастатурата за лево и десно, па затоа постои аргумент за избирање насока на движење на човечето DIRECTION. Потоа се чуваат неговите координати X и Y, кои ќе се потребни за соодветната позиција на човечето. Бидејќи тоа претставува слика, координатите се координати на дадената слика. Поставуваме и граници околу сликата.

Во конструкторот ги поставуваме координатите X=150, и Y=130 како стартни за позиција на сликата. На тоа место ќе стои човечето секогаш при стартување на играта.
Со останатите методи од оваа класа се опишува движењето на човечето, односно менување на неговата местоположба со соодветен клик на тастатурата за лево или десно.

public man()
        {
            X = 150;
            Y = 300;
            coveche = Resources.man;
            Direction = DIRECTION.LEFT;
           
        }

        public void ChangeDirection(DIRECTION direction)
        {
            Direction = direction;
        }


        public void Move(int width, int height)
        {
            if (Direction == DIRECTION.RIGHT)
                {
                X += 1;
                if (X >= width)
                {
                    X = width - 1;
                }
            }
            if (Direction == DIRECTION.LEFT)
            {
                X -= 1;
                if (X < 0)
                {
                    X = 0;
                }
            }

        }
 

Исто така една од битните класи во оваа игра е класата за балони, именувана како Balloon ,со соодветните елементи, а во формата Form1, се чува листа од овие балони. 
Light = 5 balloons
Moderate = 9 balloons
Brutal = 14 balloons


public class Balloon
    {
        
        public float X { get; set; }
        public float Y { get; set; }

        public int Radius { get; set; }
        public float Angle { get; set; }
        public float Velocity { get; set; }

        public Brush brush { get; set; }

        public Rectangle Bounds;

        private float velocityX;
        private float velocityY;

        public Point Center;


Слично како кај man и тука се чуваат координатите на балонот X и Y,центар кој ги содржи X и Y ,неговиот радиус,аголот, и брзината на движење.Постои  и некоја боја со која се избоени балончињата, како и брзина на движење по X и Y.Околу секој балон постои граница  во  која можат да се движат балоните ,тие не смеат да се движат во делот каде што се наоѓа човечето,само во горниот дел од формата.
Потоа се опишани констуркторот,методот за движење на балоните и метод за нивно исцртување. 

public Balloon(float x, float y, int radius, float velocity, float angle, Brush br, Point cc)
        {
            Center = cc;
            X = x;
            Y = y;
            Radius = radius;
            Velocity = velocity;
            Angle = angle;
            velocityX = (float)Math.Cos(Angle) * Velocity;
            velocityY = (float)Math.Sin(Angle) * Velocity;
            brush = br;
          }

        public void Move()
        {
            float nextX = X + velocityX;
            float nextY = Y + velocityY;
            if (nextX - Radius <= Bounds.Left || (nextX + Radius >= Bounds.Right))
            {
                velocityX = -velocityX;
            }
            if (nextY - Radius <= Bounds.Top || (nextY + Radius >= Bounds.Bottom))
            {
                velocityY = -velocityY;
            }
            X +=( velocityX);
            Y +=( velocityY);
        }

      
  public void Draw(Graphics g)
        {

            g.FillEllipse(brush, X - Radius, Y - Radius, Radius * 2, Radius * 2);

            Random rr = new Random();
            Pen pn = new Pen(Color.Red, 2);
            Color color = Color.FromArgb(rr.Next(255), rr.Next(255), rr.Next(255));
            Brush br = new SolidBrush(color);
        
            g.FillEllipse(br, X - Radius, Y - Radius, Radius * 2, Radius * 2);

            Point point1t = new Point((int)(X - Radius), (int)(Y));
            Point point2t = new Point((int)(X + Radius), (int)(Y));
            Point point3t = new Point((int)(X), (int)(Y + 2.5 * Radius));
            Point[] triagolnik1 = { point1t, point2t, point3t };
            g.FillPolygon(br, triagolnik1);


            g.DrawLine(pn, (int)(X), (int)(Y + 2.5 * Radius), (int)(X + 7), (int)(Y + 2.5 * Radius + 11));
            g.DrawLine(pn, (int)(X + 7), (int)(Y + 2.5 * Radius + 11), (int)(X - 5), (int)(Y + 2.5 * Radius + 21));
            g.DrawLine(pn, (int)(X - 5), (int)(Y + 2.5 * Radius + 21), (int)(X + 2), (int)(Y + 2.5 * Radius + 33));
        }

Тука интересен метод е исцртувањето на балоните,бидејќи потребно е тие да имаат  своја рандом позиција и различна боја , а и нивната форма која е составена од една кружница,триаголник и линии кои ги красат балончињата.
Додека пак нивното исцртувањето на балоните,како и останатите елементи од играта постојано се повикува преку слдниов метод:

private void DrawObjects(Graphics g)
        {
            g.Clear(Color.White);
            foreach (Balloon balonchence in balloons)
            {
                balonchence.Bounds = new Rectangle(0, 25, this.Bounds.Width, this.Bounds.Height / 2 - 35);
                balonchence.Draw(g); // go crta baloncheto
                balonchence.Move();
                graphics.DrawRectangle(pen, bound3);
                g.DrawImageUnscaled(covecheImage, CoveceX, CoveceY);
            }
            covece.Bounds = new Rectangle(0, 25, this.Bounds.Width, this.Bounds.Height / 2 - 35);
                        g.FillRectangle(groundColor, 0, this.Bounds.Height - this.Bounds.Height / 5, this.Bounds.Width, this.Bounds.Height / 4);
            topchence.Drawtopche(g);
        }


Играта е регулирана од страна на тајмерот,кој што со секое стартување на нова игра или нов левел,започнува да брои од почеток.Истекувањето на времето од тајмерот е опишано во progress Bar,максималното време кое го имате за една игра е  60 секунди ,при што  progress bar-от почнува да ја менува својата боја во последните 20 секунди од времето,со што ве потсетува дека ви преостанува уште малку време за играње на играта.
Тоа е опишано преку следниот метод:
private void timerBlink_Tick(object sender, EventArgs e)
        {
            if (pominatoVreme >= 40)
            {
                if (progressBar1.ForeColor != Color.Red)
                    progressBar1.ForeColor = Color.Red;
                else progressBar1.ForeColor = Color.Yellow;
            }

        }

Покрај progress Bar во долниот дел од формата се наоѓаат  две лабели, едната  кажува колку балони успешно сте  погодиле и уште една лабела која ви ги кажува вашите постигнати поени соодветно со секој погодок на балонот, бројот на поени ви се зголемува за 2. Постои и лабела во toolStrip која го кажува бројот на вкупните обиди за пукање на балоните.
Рамнината на која стои и се движи човекот на почетокот е бела. Kако што одминува времето таа станува се поцрвена, за кога времето ќе истече да стане целосно црвена. Црвенеењето на подлогата му иницира на играчот дека преминува во поопасна зона односно треба да ги пукне балончињата, во спротивно доколу не успее се појавува соодветен MessageBox кој кажува дека ви измина времето, тајмерот се стопира, или пак доколку успешно ја изиграте играта ви се појавува нов МеssageBox кој ви честита за успешино изиграниот левел, и ви дава можност за избирање на нов левел и нова игра.

Задачата на корисникот е да успее да ги испука балоните без да му истече времето. Тоа се прави со кликање на маусот на соодветната позиција на балонот, меѓутоа, мора корисникот да го следи претходното движење на балоните, и да ја запази нивната следна  позиција, која е лесно видлива заради очигледната траекторија по која се движат балоните. Се разбира, позицијата на балоните се менува со соодветно со тикот на тајмерот. Со соодветен клик кон балонот се исцртува линија.
За оваа линија која се исцртува можете сами произволно да си избирате дебелина и боја преку изборот за можности кој ви го дава menustrip.  



