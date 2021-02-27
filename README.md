Doctrine Array to Entity Hydrator
================

Introduction
------------


A hydrator for doctrine 2 that converts an array to the entity of your choice.

Installing via Composer
-----------------------

The recommended way to install is through
[Composer](http://getcomposer.org).

    composer require solodkiy/doctrine-array-hydrator

Example
-------

Given this doctrine entity:

```PHP
<?php

namespace App\Entity;
    
use Doctrine\ORM\Mapping as ORM;
use Solodkiy\Doctrine\Hydrator\Test\Fixture\Company;
use Solodkiy\Doctrine\Hydrator\Test\Fixture\Permission;

/**
 * @ORM\Entity
 * @ORM\Table(name="users")
 */
class User
{
   /**
    * @ORM\Id 
    * @ORM\Column(type="integer") 
    * @ORM\GeneratedValue
    * 
    * @var int
    */
    protected $id;
    
    /**
     * @ORM\Column(type="string")
     * 
     * @var string
     */
    protected $name;
    
    /**
     * @ORM\Column(type="string")
     * 
     * @var string
     */
    protected $email;
    
    /**
     * @ManyToOne(targetEntity="Company")
     * 
     * @var Company
     */
    protected $company;
        
    /**
     * @OneToMany(targetEntity="Permission", mappedBy="product")
     * 
     * @var Permission[]
     */
    protected $permissions;
}
```

We can populate this object with an array, for example:

```PHP
$data = [
    'name'        => 'Fred Jones',
    'email'       => 'fred@example.com',
    'company'     => 2,
    'permissions' => [1, 2, 3, 4]
];

$hydrator = new \Solodkiy\Doctrine\Hydrator\ArrayHydrator($entityManager);
$entity   = $hydrator->hydrate('App\Entity\User', $data);
```

We can populate user with JSON API resource data
[Documentation](http://jsonapi.org/format/#document-resource-objects)
```PHP
$data = [
    'attributes'    => [
        'name'  => 'Fred Jones',
        'email' => 'fred@example.com',
    ],
    'relationships' => [
        'company'     => [
            'data' => ['id' => 1, 'type' => 'company'],
        ],
        'permissions' => [
            'data' => [
                ['id' => 1, 'type' => 'permission'],
                ['id' => 2, 'type' => 'permission'],
                ['id' => 3, 'type' => 'permission'],
                ['id' => 4, 'type' => 'permission'],
                ['name' => 'New permission']
            ]
        ]
    ]
];
    
$hydrator = new \Solodkiy\Doctrine\Hydrator\JsonApiHydrator($entityManager);
$entity   = $hydrator->hydrate('App\Entity\User', $data);
```

Copyright
---------

Doctrine Array to Entity Hydrator  
Copyright (c) 2015 pmill (dev.pmill@gmail.com)   
Copyright (c) 2021 Alexey Solodkiy  
All rights reserved.
