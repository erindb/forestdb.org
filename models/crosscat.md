---
layout: model
title: CrossCat
model-status: code
model-category: Miscellaneous
model-tags: nonparametric models, categorization
---

CrossCat is a generative model that can be used for categorizing
objects given their features. The model assumes that each data
point is generated using multiple systems of categories; each such
system accounts for some subset of the features [@Shafto:2006tz;
@churchwiki].

    (query
    
     (define kind-distribution (DPmem 1.0 gensym))
    
     (define feature->kind
       (mem (lambda (feature) (kind-distribution))))
    
     (define kind->class-distribution
       (mem (lambda (kind) (DPmem 1.0 gensym))))
    
     (define feature-kind/object->class
       (mem (lambda (kind object) (sample (kind->class-distribution kind)))))
    
     (define class->parameters
       (mem (lambda (object-class) (first (beta 1 1)))))
    
     (define (observe object feature)
       (flip (class->parameters (feature-kind/object->class (feature->kind feature) object))))
    
     (list (observe 'spinach 'breakfast)
           (observe 'eggs 'breakfast))
    
     (and (observe 'eggs 'breakfast)
          (observe 'toast 'breakfast)
          (observe 'eggs 'dinner)
          (observe 'spinach 'dinner)))