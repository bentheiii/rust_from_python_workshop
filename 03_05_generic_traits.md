traits can be generic themselves
```rust
trait Measurement {}

struct Distance; // iso- meters
impl Measurement for Distance{}
struct Temperature;  // iso- kelvin
impl Measurement for Temperature{}

trait Unit<M: Measurement>{
    fn to_iso(&self)->f64;
    fn from_iso(iso: f64)->Self;
}

#[derive(Debug)]
struct Meters(f64);
#[derive(Debug)]
struct Feet(f64);

impl Unit<Distance> for Meters{
    fn to_iso(&self)->f64{
        return self.0
    }
    fn from_iso(iso: f64)->Self{
        return Self(iso)
    }
}

impl Unit<Distance> for Feet{
    fn to_iso(&self)->f64{
        return self.0 * 0.3048
    }
    fn from_iso(iso: f64)->Self{
        return Self(iso/0.3048)
    }
}

#[derive(Debug)]
struct Kelvin(f64);
#[derive(Debug)]
struct Celsius(f64);
#[derive(Debug)]
struct Fahrenheit(f64);

impl Unit<Temperature> for Kelvin{
    fn to_iso(&self)->f64{
        return self.0
    }
    fn from_iso(iso: f64)->Self{
        return Self(iso)
    }
}

impl Unit<Temperature> for Celsius{
    fn to_iso(&self)->f64{
        return self.0 + 273.15
    }
    fn from_iso(iso: f64)->Self{
        return Self(iso - 273.15)
    }
}

impl Unit<Temperature> for Fahrenheit{
    fn to_iso(&self)->f64{
        return (self.0 + 459.67)*5.0/9.0
    }
    fn from_iso(iso: f64)->Self{
        return Self(iso*9.0/5.0  - 459.67)
    }
}

fn from_unit<M: Measurement, In: Unit<M>, Out: Unit<M>>(i: In)->Out{
    Out::from_iso(i.to_iso())
}

fn main() {
    let a_hundred_meters_in_feet: Feet = from_unit(Meters(100.0));
    println!("{a_hundred_meters_in_feet:?}");  // Feet(328.0839895013123)
    let boiling_water_fahrenheit: Fahrenheit = from_unit(Celsius(100.0));
    println!("{boiling_water_fahrenheit:?}");  // Fahrenheit(211.99999999999994)
    
    let a_mile_in_kelvin: Kelvin = from_unit(Feet(5280.0)); // compilation error!
}
```