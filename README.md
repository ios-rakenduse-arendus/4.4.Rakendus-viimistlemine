
# 4.4.Rakendus: Viimistlemine


1. Tuvastame kokkupuuted ja kokkupõrked erinevate füüsiliste objektide vahel. Avame *GameScene.swift* fail ning kirjutame sinna järgmise funktsiooni:

```swift
/* Kontrollime Eksmati kokkupõrget posti või põrandaga ning nendel juhtudel lõpetame mängu */
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
            
/* Kontrollime, kas Eksmati sai EAP kätte, lisame sellele tegevusele heli ning suurendame skoori ja eemaldame kätte saadud EAP ekraanilt*/
        } else if firstBody.categoryBitMask == CollisionBitMask.Category && secondBody.categoryBitMask == CollisionBitMask.EAPCategory {
            run(coinSound)
            score += 1
            scoreLbl.text = "\(score)"
            secondBody.node?.removeFromParent()
        } else if firstBody.categoryBitMask == CollisionBitMask.EAPCategory && secondBody.categoryBitMask == CollisionBitMask.eksmatiCategory {
            run(coinSound)
            score += 1
            scoreLbl.text = "\(score)"
            firstBody.node?.removeFromParent()
        }
}
```

### 2. Lisame restardi, kui Eksmati kaotab. Loo *GameScene.swift* faili funktsioon nimega ```restartScene``` järgmiselt:

func restartScene(){
    self.removeAllChildren()
    self.removeAllActions()
    isDied = false
    isGameStarted = false
    score = 0
    createScene()
}

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
	* Esiteks alla tõmmata siit samast repositooriumist(üleval) kättesaadav fail nimega EAPSound.mp3
	* Lisada see fail Xcode'is EKSMATI projektis EKSMATI nimelisse kausta
	* Järgmiseks tuleb lisada esimeses praktilises tunnis (**4.1. Rakendus: Projekti algus & tausta loomine**) kirjutatud muutujate hulka järgmine koodiriba: 

```let coinSound = SKAction.playSoundFileNamed("EAPSound.mp3", waitForCompletion: false)```

>Käivita simulatsion ja kontrolli, kas äpp'is toimib kõik nii nagu peab ning EAP'de kogumisel tuleb nüüd ka heli.

* **Vaata nüüd, kas oskad iseseisvalt välja mõelda kuidas lisada mängule siin repositooriumis olemasolev wolfSound.mp3 heli nii, et see mängiks, kui Eksmati surma saab.**
___


# TUBLI! Sellega sai valmis meie Eksmati äpp.
