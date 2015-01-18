---
layout: default
---

# Unrestricted Connectivity

![banking]({{ site.github.url }}/part2/images/uml_banking.jpg)

Om multiple-assiciation te hebben gebruiken we een lijst structuur. Met bijhorende `getXAt(i)`, `addAsXAt(x,i)`, `removeAsXAt(i)`. Alternatief kan je ook `addAsX(x)` en `removeAsX(x)` gebruiken als ze ongeordend zijn. Daar komen dan nog checkers bij `canHaveAsXAt(x,i)` of `canHaveAsX(x)`. En introduceer een class invariant `hasProperXs()` (meervoud).

Om een of andere reden moet je dit 1-based doen, en niet 0-based. Dus als je de getter implementeerd doe je:

{% highlight java %}
@Basic
@Raw
public Purchase getPurchaseAt(int index) throws IndexOutOfBoundsException {
    return purchases.get(index - 1);
}
{% endhighlight %}

<!--more-->

## Voorbeeldje van alle nodige methodes voor unrestricted connectivity

* getGranteeAt(i)
* getNbGrantees()
* addAsGranteeAt(person,i)
* addAsGrantee(person)
* removeAsGranteeAt(i)
* removeAsGrantee(person)
* canHaveAsGranteeAt(person,i)
* hasProperGrantees()

of ongeordend:

* hasAsSavingsAccount(savings)
* addAsSavingsAccount(savings)
* removeAsSavingsAccount(savings)
* canHaveAsSavingsAccount(savings)
* hasProperSavingsAccounts()

## Voorbeeld van een classe

Een deel van de class is weggehaald. Enkel de delen die de meerdere-associatie tonen zijn gebleven.

{% highlight java %}

package banking.shares;

import java.util.*;
import banking.money.MoneyAmount;
import banking.shares.Purchase;
import be.kuleuven.cs.som.annotate.*;

/**
* A class a shares involving a code, a value and a set of
* purchases in which they are involved.
*
* @invar   Each share must have a valid code.
*        | canHaveAsCode(getCode())
* @invar   The value of each share must be a valid value for any share.
*        | isValidValue(getValue())
* @invar   Each share must have proper purchases.
*        | hasProperPurchases()
*
* @version 2.0
* @author  Eric Steegmans
*/
public class Share {

    /**
    * Return the purchase associated with this share at the
    * given index.
    *
    * @param  index
    *         The index of the purchase to return.
    * @throws IndexOutOfBoundsException
    *         The given index is not positive or it exceeds the
    *         number of purchases for this share.
    *       | (index < 1) || (index > getNbPurchases())
    */
    @Basic
    @Raw
    public Purchase getPurchaseAt(int index) throws IndexOutOfBoundsException {
        return purchases.get(index - 1);
    }

    /**
    * Return the number of purchases associated with this share.
    */
    @Basic
    @Raw
    public int getNbPurchases() {
        return purchases.size();
    }

    /**
    * Check whether this share can have the given purchase
    * as one of its purchases.
    *
    * @param  purchase
    *         The purchase to check.
    * @return True if and only if the given purchase is effective
    *         and that purchase can have this share as its share.
    *       | result ==
    *       |   (purchase != null) &&
    *       |   purchase.canHaveAsShare(this)
    */
    @Raw
    public boolean canHaveAsPurchase(Purchase purchase) {
        return (purchase != null) && (purchase.canHaveAsShare(this));
    }

    /**
    * Check whether this share can have the given purchase
    * as one of its purchases at the given index.
    *
    * @param  purchase
    *         The purchase to check.
    * @return False if the given index is not positive or exceeds the
    *         number of purchases for this share + 1.
    *       | if ( (index < 1) || (index > getNbPurchases()+1) )
    *       |   then result == false
    *         Otherwise, false if this share cannot have the given
    *         purchase as one of its purchases.
    *       | else if ( ! this.canHaveAsPurchase(purchase) )
    *       |   then result == false
    *         Otherwise, true if and only if the given purchase is
    *         not registered at another index than the given index.
    *       | else result ==
    *       |   for each I in 1..getNbPurchases():
    *       |     (index == I) || (getPurchaseAt(I) != purchase)
    */
    @Raw
    public boolean canHaveAsPurchaseAt(Purchase purchase, int index) {
        if ((index < 1) || (index > getNbPurchases() + 1))
        return false;
        if (!this.canHaveAsPurchase(purchase))
        return false;
        for (int i = 1; i < getNbPurchases(); i++)
        if ((i != index) && (getPurchaseAt(i) == purchase))
        return false;
        return true;
    }

    /**
    * Check whether this share has proper purchases attached to it.
    *
    * @return True if and only if this share can have each of the
    *         purchases attached to it as a purchase at the given index,
    *         and if each of these purchases references this share as
    *         the share to which they are attached.
    *       | result ==
    *       |   for each I in 1..getNbPurchases():
    *       |     ( this.canHaveAsPurchaseAt(purchase,I) &&
    *       |       (purchase.getShare() == this) )
    */
    public boolean hasProperPurchases() {
        for (int i = 1; i <= getNbPurchases(); i++) {
            if (!canHaveAsPurchaseAt(getPurchaseAt(i), i))
            return false;
            if (getPurchaseAt(i).getShare() != this)
            return false;
        }
        return true;
    }

    /**
    * Check whether this share has the given purchase as one of its
    * purchases.
    *
    * @param  purchase
    * 		   The purchase to check.
    * @return The given purchase is registered at some position as
    *         a purchase of this share.
    *       | for some I in 1..getNbPurchases():
    *       |   getPurchaseAt(I) == purchase
    */
    public boolean hasAsPurchase(@Raw Purchase purchase) {
        return purchases.contains(purchase);
        // A more efficient implementation would be possible if
        // the consistency imposed on the bi-directional association
        // would be guaranteed.
        // return (purchase != null) && (purchase.getShare() == this);
    }

    /**
    * Add the given purchase to the list of purchases of this share.
    *
    * @param  purchase
    *         The purchase to be added.
    * @pre    The given purchase is effective and already references
    *         this share, and this share does not yet have the given
    *         purchase as one of its purchases.
    *       | (purchase != null) && (purchase.getShare() == this) &&
    *       | (! this.hasAsPurchase(purchase))
    * @post   The number of purchases of this share is
    *         incremented by 1.
    *       | new.getNbPurchases() == getNbPurchases() + 1
    * @post   This share has the given purchase as its very last purchase.
    *       | new.getPurchaseAt(getNbPurchases()+1) == purchase
    */
    void addPurchase(@Raw Purchase purchase) {
        assert (purchase != null) && (purchase.getShare() == this)
        && (!this.hasAsPurchase(purchase));
        purchases.add(purchase);
    }

    /**
    * Remove the given purchase from the list of purchases of this share.
    *
    * @param  purchase
    *         The purchase to be removed.
    * @pre    The given purchase is effective, this share has the
    *         given purchase as one of its purchases, and the given
    *         purchase does not reference any share.
    *       | (purchase != null) &&
    *       | this.hasAsPurchase(purchase) &&
    *       | (purchase.getShare() == null)
    * @post   The number of purchases of this share is
    *         decremented by 1.
    *       | new.getNbPurchases() == getNbPurchases() - 1
    * @post    his share no longer has the given purchase as
    *         one of its purchases.
    *       | ! new.hasAsPurchase(purchase)
    * @post   All purchases registered at an index beyond the index at
    *         which the given purchase was registered, are shifted
    *         one position to the left.
    *       | for each I,J in 1..getNbPurchases():
    *       |   if ( (getPurchaseAt(I) == purchase) and (I < J) )
    *       |     then new.getPurchaseAt(J-1) == getPurchaseAt(J)
    */
    @Raw
    void removePurchase(Purchase purchase) {
        assert (purchase != null) && this.hasAsPurchase(purchase)
        && (purchase.getShare() == null);
        purchases.remove(purchase);
    }

    /**
    * Variable referencing a list collecting all the purchases in
    * which this share is involved.
    *
    * @invar  The referenced list is effective.
    *       | purchases != null
    * @invar  Each purchase registered in the referenced list is
    *         effective and not yet terminated.
    *       | for each purchase in purchases:
    *       |   ( (purchase != null) &&
    *       |     (! purchase.isTerminated()) )
    * @invar  No purchase is registered at several positions
    *         in the referenced list.
    *       | for each I,J in 0..purchases.size()-1:
    *       |   ( (I == J) ||
    *       |     (purchases.get(I) != purchases.get(J))
    */
    private final List<Purchase> purchases = new ArrayList<Purchase>();

}

{% endhighlight %}
