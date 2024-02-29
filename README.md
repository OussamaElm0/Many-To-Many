
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

# Insert in Intermediate Table

## Method 1

```php
$stagiaire = Stagiaire::find($request->stagiaire);
$stagiaire->modules()
        ->attach($request->module, ['note' => $request->note])
```
* note : Column in the pivot table

