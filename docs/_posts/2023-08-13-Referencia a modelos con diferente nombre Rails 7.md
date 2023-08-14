---
layout: single
toc: true
title: "Agregar una referencia con nombre de modelo distinto en Rails 7"
date: 2023-07-11
categories:
  - Rails 7
  - Active Model
---

Para agregar una referencia a un modelo donde la referencia es nombrada de forma distinta al modelo se puede hacer lo siguiente:

{% highlight bash %}
rails g migration Add<Referencia>To<Modelo a Referenciar> <Nombre Campo>:references
{% endhighlight %}

Por ejemplo, para un modelo a referenciar `AccountType` y el modelo que referencia llamado `Account`:

{% highlight bash %}
rails g migration AddTypeToAccount type:references
{% endhighlight %}

Y luego tenemos que editar la migración creada como sigue:

{% highlight ruby %}
class AddTypeToAccount < ActiveRecord::Migration
  def change
    add_reference :type, :account, foreign_key: { to_table: :account_type }
  end
end
{% endhighlight %}

Lo importante es cambiar el `foreignkey: true ` a `foreign_key: { to_table: :tabla_a_referenciar }`

Luego para terminar debes especificar la relación de forma correcta en el modelo: 

{% highlight ruby %}
class Account < ApplicationRecord
  belongs_to :type, class_name: 'AccountType'
end

class AccountType < ApplicationRecord
  has_many :accounts, foreign_key: 'type_id'
end
{% endhighlight %}