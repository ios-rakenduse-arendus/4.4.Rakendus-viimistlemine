
# 4.4.Rakendus: Viimistlemine


### 1. Tuvastame kokkupuuted ja kokkupõrked erinevate füüsiliste objektide vahel. Avame *GameScene.swift* fail ning kirjutame sinna järgmise funktsiooni:

```swift

func didBegin(_ contact: SKPhysicsContact) {
        let firstBody = contact.bodyA
        let secondBody = contact.bodyB
        
        if firstBody.categoryBitMask == CollisionBitMask.eksmatiCategory && secondBody.categoryBitMask == CollisionBitMask.postCategory || firstBody.categoryBitMask == CollisionBitMask.postCategory && secondBody.categoryBitMask == CollisionBitMask.eksmatiCategory || firstBody.categoryBitMask == CollisionBitMask.eksmatiCategory && secondBody.categoryBitMask == CollisionBitMask.groundCategory || firstBody.categoryBitMask == CollisionBitMask.groundCategory && secondBody.categoryBitMask == CollisionBitMask.eksmatiCategory{
            enumerateChildNodes(withName: "wallPair", using: ({
                (node, error) in
                node.speed = 0
                self.removeAllActions()
            }))
            if isDied == false{
                isDied = true
                createRestartBtn()
                pauseBtn.removeFromParent()
                self.eksmati.removeAllActions()
            }
            

        } else if firstBody.categoryBitMask == CollisionBitMask.eksmatiCategory && secondBody.categoryBitMask == CollisionBitMask.EAPCategory {
            run(EAPSound)
            score += 1
            scoreLbl.text = "\(score)"
            secondBody.node?.removeFromParent()
        } else if firstBody.categoryBitMask == CollisionBitMask.EAPCategory && secondBody.categoryBitMask == CollisionBitMask.eksmatiCategory {
            run(EAPSound)
            score += 1
            scoreLbl.text = "\(score)"
            firstBody.node?.removeFromParent()
        }
}
```

### 2. Lisame restardi, kui Eksmati kaotab. Loo *GameScene.swift* faili funktsioon nimega ```restartScene``` järgmiselt:

```swift
func restartScene(){
    self.removeAllChildren()
    self.removeAllActions()
    isDied = false
    isGameStarted = false
    score = 0
    createScene()
}
```

### 3. Kirjuta ```touchesBegan``` funktsiooni sisse järgmine kood:

```swift

/* Kontrollime, kus ja milla mängija ekraani vajutas, et me teaks, mida mäng parajasti kuvama peab */
for touch in touches{
    let location = touch.location(in: self)
    
/* Sündmused, kui mängija kaotas: kuvame ainut restart nupu ning kontrollime selle vajutamisel parimat skoori */
    if isDied == true{
        if restartBtn.contains(location){
            if UserDefaults.standard.object(forKey: "highestScore") != nil {
                let hscore = UserDefaults.standard.integer(forKey: "highestScore")
                if hscore < Int(scoreLbl.text!)!{
                    UserDefaults.standard.set(scoreLbl.text, forKey: "highestScore")
                }
            } else {
                UserDefaults.standard.set(0, forKey: "highestScore")
            }
            restartScene()
        }
    } else {     
        
/* Sündmused, kui mäng veel kestab: pausi ja play nupu ja sündmuse kuvamine */
        if pauseBtn.contains(location){
            if self.isPaused == false{
                self.isPaused = true
                pauseBtn.texture = SKTexture(imageNamed: "play")
            } else {
                self.isPaused = false
                pauseBtn.texture = SKTexture(imageNamed: "pause")
            }
        }
    }
}
```

# ÜLESANNE:
* Selleks, et tekiks heli, kui Eksmati korjab EAP'sid tuleb :
	* Esiteks alla tõmmata siit samast repositooriumist(üleval) kättesaadav kausta .zip file nimega **helid**
	* Lisada seal olev EAPSound.mp3 Xcode'is EKSMATI projektis EKSMATI nimelisse kausta
	* Järgmiseks tuleb lisada esimeses praktilises tunnis (**4.1. Rakendus: Projekti algus & tausta loomine**) kirjutatud muutujate hulka järgmine koodiriba: 

```let EAPSound = SKAction.playSoundFileNamed("EAPSound.mp3", waitForCompletion: false)```

>Käivita simulatsion ja kontrolli, kas äpp'is toimib kõik nii nagu peab ning EAP'de kogumisel tuleb nüüd ka heli.

* **Vaata nüüd, kas oskad iseseisvalt välja mõelda kuidas lisada mängule siin repositooriumis heli kaustas olemasolev wolfSound.mp3 nii, et see mängiks, kui Eksmati kaotab.**
___


**VIDEO (1.,2.,3. ja esimene osa ülesannetest):**

<a href="https://youtu.be/-XcY-7mJ6Dk
" target="_blank"><img src="http://img.youtube.com/vi/-XcY-7mJ6Dk/0.jpg" 
alt="Projekti loomine" width="240" height="180" border="10" /></a>


# TUBLI! Sellega sai valmis meie Eksmati äpp.

### Ideid äpi edasi arendamiseks:

* Eksmati animeerimine
>Näiteks joonistda juurde Eksmatist erineva liikumisasendiga pildid ning sisestada need mängu nii, et kui Eksmati ülesse liigub vajutuse peale siis muudab ta oma olekut jättes näiteks mulje hüppamisest või keel tuleb suust välja ja sisse.
* Eksmatile  heli juurde lisamine
> Ehk, kui eksmati hüppab üles siis kasneb sellega ka üleshüppe heli või hundi jooksmise heli. 
* Parima skoori edetabel
* Teemade lisamine
> Näiteks öö ja päeva nupp.
