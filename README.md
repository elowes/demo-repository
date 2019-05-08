const wonderBitsSdk = require('js-web-sdk')

const randomColor = [0, 50, 100, 150, 200, 255]

const getRandomColor = () => randomColor[Math.floor(Math.random() * randomColor.length)]

wonderBitsSdk.initConnection(async () => {
    console.log('init success')
    const moduleNames = await wonderBitsSdk.getConnectedModuleNames()
    console.log(`modules: ${moduleNames}`)
    setInterval(async () => {
        const { lightBelt, display, observer, ultrasonic } = wonderBitsSdk
        const rgb = [getRandomColor(), getRandomColor(), getRandomColor()]
        lightBelt.setLedsRgb(1, 1, 10, ...rgb)
        display.print(1, 5, 1, "hello world", 2)
        const [temperature, humidity, light, volume, distance] = await Promise.all([
            observer.getTemperature(1),
            observer.getHumidity(1),
            observer.getLight(1),
            observer.getVolume(1),
            ultrasonic.getDistance(1)
        ]) 
        console.clear()
        console.log(`color: rgb(${rgb.join(',')})`)
        console.log(`temperature: ${temperature}`)
        console.log(`humidity: ${humidity}`)
        console.log(`light: ${light}`)
        console.log(`volume: ${volume}`)
        console.log(`distance: ${distance}`)
    }, 3000);
})
