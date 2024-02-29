
# Retrieving Intermediate Table Columns

## Method 1

```php
$stagiaires = Stagiaire::find($id);
                    ->modules()
                    ->select('name', 'note')
                    ->get();
```
* Select only name's and note's columns (note is a column in the pivot table)

## Method 2 
In Module's model: 
```php
public function stagiaires()
    {
        return $this->belongsToMany(Stagiaire::class)
                ->withPivot('note');
    }
```

In Stagiaire's model:
```php
public function modules()
    {
        return $this->belongsToMany(Module::class)
                ->withPivot('note');
    }
```

In controller:
```php
$stagiaires = Stagiaire::find($id);

foreach ($stagiaires->modules as $module) :
                echo $module->name . ' : ' . $module->pivot->note . "<br>";
endforeach;
```

## Method 3 
After adding withPivot() method within models

```php
$stagiaires = Stagiaire::with("modules")-get();

foreach($stagiaires as $stagiaire):
  foreach ($stagiaire->modules as $module) :
                echo $module->name . ' : ' . $module->pivot->note . "<br>";
  endforeach;
endforeach;

```
# Insert in Intermediate Table

## Method 1

```php
$stagiaire = Stagiaire::find($request->stagiaire);
$stagiaire->modules()
        ->attach($request->module, ['note' => $request->note])
```
* note : Column in the pivot table

# Delete from Intermediate Table

```php
$stagiaire = Stagiaire::find($request->stagiaire)
$stagiaire->modules()
        ->detach($request->module)
```
* If you want to detach all items, you must keep detach's args blank
