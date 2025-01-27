
# Rapport

**Uppgift 8 - External libraries**

Uppgiften gick ut på att skapa en app som använder sig utav externa bibliotek skapade av personer som laddat upp de för allmänheten att använda. Appen använder två olika bibliotek. Ett för konfetti och ett för en progess bar anpassad för Material Design. Appen använder Material Design som utgångspunkt för utseende. Med en top bar med en primärfärg och meny knappar. 

<img src="app1.png" width="300">

Det används även fragments för att kunna navigera runt på sidan utan att behöva skapa nya aktiviteter. Alla knappar som ligger i meyn går att klicka på och är en del av menyn och en knapp-lyssnare och varje knapp har ett fragment kopplat till sig som körs när en knapp klickas på. Exempel på ett fragment:

```Java

    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        MaterialButton button = view.findViewById(R.id.materialButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(getActivity().getApplicationContext(), SecondActivity.class);
                startActivity(intent);
            }
        });
    }

```
Eftersom views inte finns när fragments laddas in och skapas. Så måste man hämta views efter fragment skapats så man kan referera och hämta vyer till den. Här körs funktionen onViewCreated som körs efteråt och då kan man t ex skapa som här en intent som laddar en ny aktivitet.

För att använda bibliotek krävs att man länkar in dessa i dependencies i gradle. 
```
    implementation 'com.google.android.material:material:1.4.0-alpha02'
    implementation 'nl.dionsegijn:konfetti:1.3.2'
    implementation 'me.zhanghai.android.materialprogressbar:library:1.6.1'
```
Dessa är för Material Design, konfetti och Material Progressbar komponenter.

Första externa biblioteket som används är konfetti här som används på alla sidor eftersom fragments ligger i MainActivity så kan konfetti användas på alla sidor/fragments.

Koden som skrävs för att skapa konfetti på sidan:

```
<nl.dionsegijn.konfetti.KonfettiView
        android:id="@+id/viewKonfetti"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
```

```Java
 final Drawable drawable = ContextCompat.getDrawable(getApplicationContext(), R.drawable.ic_favorite_24);
        final Shape.DrawableShape drawableShape = new Shape.DrawableShape(drawable, true);

        final KonfettiView konfettiView = findViewById(R.id.viewKonfetti);
        MaterialToolbar toolbar = findViewById(R.id.topAppBar);
        setSupportActionBar(toolbar);
        toolbar.setNavigationOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                konfettiView.build()
                        .addColors(Color.YELLOW, Color.GREEN, Color.MAGENTA, Color.BLUE)
                        .setDirection(0.0, 359.0)
                        .setSpeed(1f, 5f)
                        .setFadeOutEnabled(true)
                        .setTimeToLive(2000L)
                        .addShapes(Shape.Square.INSTANCE, Shape.Circle.INSTANCE, drawableShape)
                        .addSizes(new Size(12, 5f))
                        .setPosition(-50f, konfettiView.getWidth() + 50f, -50f, -50f)
                        .streamFor(300, 5000L);
            }
        });
```
Det är som vanligt viktigt att man kopplar den till rätt id. Här skapas konfetti när man trycker på menyknappen. Det skapas konstanter av drawableShape och konfettiView detta enligt biliotekets rekommendationer. När man trycker på knappen skapas nya konfetti som regnar ner över skärmen.

<img src="app3.png" width="300">

I en fragments skapas en Material Progress Bar som också är ett extern bibliotek. Som skapar laddningsanimationer i olika former, cirklar eller horisontellt. Här är implementationen mycket enklare och krävs endast man skapar en vy i en layout fil.

```
<me.zhanghai.android.materialprogressbar.MaterialProgressBar
            android:layout_gravity="center"
            style="@style/Widget.MaterialProgressBar.ProgressBar.Horizontal"
            android:layout_width="350dp"
            android:layout_height="350dp"
            android:indeterminate="true"
            app:mpb_progressStyle="circular"
            android:theme="@style/AppTheme"/>
```
Detta är endast som krävs för att implementera en MaterialProgessBar sen går det ändra i Javakod om man vill. Det ser ut så här:

<img src="app4.png" width="300">

Appen använder olika Material Design komponenter som inte är en del utav uppgiften utan mest för att lära sig Android och lära sig designspråket.

<img src="app7.png" width="300"><img src="app5.png" width="300"><img src="app6.png" width="300">

När man klickar på Button 1 så skapas en ny aktivitet. Denna aktivitet har en toolbar med en bakåt knapp. När den klickas på avslutas aktiviteten med finish().
